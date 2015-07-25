---
title: How to secure a Rails app on heroku with SSL (Firesheep)
author: jhund
layout: post
permalink: /2010/11/how-to-secure-a-rails-app-on-heroku-with-ssl-firesheep/
other_media:
  - http://clearcove.ca/wp-content/uploads/2010/11/firesheep_screenshot.png
main_video:
  - 
categories:
  - Note to self
  - Ruby/Rails
---
The [Firesheep][1] Firefox plugin got a lot of attention lately, and rightly so. It raised awareness for the issue of HTTP session hijacking. It&#8217;s about time we make the internet a more secure place.

I just secured one of our soon to be launched products ([Tiro][2]) and am documenting the process here. When you follow this tutorial, you will get a Rails app fully secured against HTTP session hijacking as illustrated by the [Firesheep][1] Firefox plugin.

<!--more-->

At the end of this tutorial, you will have:

  * A Rails app that uses SSL for every request (assets included)
  * Secure Rails and Authlogic authentication cookies that are only transmitted via SSL connections, so they are never sent in plain text

For this tutorial, we assume the following conditions:

  * Rails3
  * hosted on heroku (make sure your app already runs on heroku via http)
  * using heroku&#8217;s &#8220;Hostname Based Custom SSL&#8221; ($20.00 per month, NOTE: works on subdomains only)
  * all cookies marked as &#8220;secure&#8221;, even the authlogic persistence one
  * dev machine is OS X, however this is easily transferrable to other OSs

This is a pretty long procedure, so let&#8217;s start with a high level overview:

  1. Buy a certificate
  2. Set up the subdomain and certificate on heroku, update DNS
  3. Secure your Rails app by allowing SSL requests only via Rack middleware
  4. Mark Rails session cookies as &#8220;secure&#8221;
  5. Mark Authlogic persistence cookies as &#8220;secure&#8221;

For this post, I use my.tiroapp.com as an example subdomain. I assume that you want to lock down a single subdomain. Multiple subdomains or a top-level domain require slightly different procedures that are not covered here. So whenever you see my.tiroapp.com here, replace it with your own host name.

Alright, let&#8217;s start&#8230;

## Buy a certificate

I bought mine for $13 at GoDaddy. To get that deal, simply google for &#8220;godaddy ssl&#8221;. Then click on the ad. This certificate is normally $50, so that&#8217;s a pretty good deal.

Once you have purchased it, you need to go to your GoDaddy dashboard and generate the certificate with the credit you purchased in the previous step.

When you start the process of generating a certificate, you&#8217;ll get to a point where the form asks for a CSR (Certificate Signing Request). Here is how to generate one:

  1. cd into your Rails app root
  2. <pre>> mkdir ssl-cert</pre>

  3. <pre>> cd ssl-cert</pre>

  4. <pre>> openssl genrsa -des3 -out host.key 2048</pre>

  5. enter and record a passphrase (we will remove it later, but we&#8217;ll need it again)
  6. <pre>> openssl req -new -key host.key -out host.csr</pre>

  7. answer all the questions, but be very careful to use &#8220;my.tiroapp.com&#8221; under the &#8220;organizational unit name&#8221; and &#8220;common name&#8221; fields

Here are example answers for the questions:

<pre>Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:California
Locality Name (eg, city) []:Mountain View
Organization Name (eg, company) [Internet Widgits Pty Ltd]: My Company Name
Organizational Unit Name (eg, section) []:my.tiroapp.com
Common Name (eg, YOUR name) []:my.tiroapp.com
Email Address []:contact@tiroapp.com

Please enter the following ‘extra’ attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:

</pre>

.

We have now generated a Certificate Signing Request. Let&#8217;s use it at GoDaddy:

  1. copy the contents of host.csr and paste them into godaddy&#8217;s form, where they ask for your certificate signing request.
  2. download the certificate from godaddy and unzip the two contained files into the ssl-cert folder in your rails app.
  3. combine the two files: cd into the ssl-cert folder
  4. <pre>> cat my.tiroapp.com.crt gd_bundle.crt > tiroapp_combined.crt</pre>

  5. <pre>> cat my.tiroapp.com.crt host.key > host.pem</pre>

Ok, now we have a certificate. In the next section we will remove the passphrase, as required by Heroku so that they can restart the server. Here you will need the passphrase I asked you to record earlier.

  1. cd into the ssl-cert folder in your rails app
  2. <pre>> openssl rsa -in host.pem -out nopassphrase.pem</pre>

  3. <pre>> openssl x509 -in host.pem >>nopassphrase.pem</pre>

  4. <pre>> openssl rsa -in host.key -out nopassphrase.key</pre>

With the passphrase removed, we can now set up things at heroku.

## Set up the subdomain and certificate on heroku

Adding a &#8220;Hostname Based Custom SSL&#8221; to heroku costs $20 per month. The following commands will trigger that additional charge:

  1. cd into the rails root of your application
  2. <pre>heroku ssl:add ssl-cert/nopassphrase.pem ssl-cert/nopassphrase.key</pre>

  3. <pre>heroku addons:add custom_domains:basic</pre>

  4. <pre>heroku domains:add my.tiroapp.com</pre>

  5. <pre>heroku addons:add ssl:hostname</pre>

Once you are done with this, heroku will email you a CNAME to use for your subdomain. This CNAME has to be used instead of the regular proxy.heroku.com. Go to your DNS provider and create/update the CNAME for my.tiroapp.com. The new CNAME will be similar to:

<pre>appid1234herokucom-56789.us-east-1.elb.amazonaws.com</pre>

## Secure your Rails app

Now that we have the certificate in place, we want to force all requests (app and assets) to use SSL. Rack is perfectly suited for this purpose:

<pre class="brush: ruby; title: ; notranslate" title=""># lib/middleware/force_ssl.rb
class ForceSSL
  def initialize(app)
    @app = app
  end

  def call(env)
    if env['HTTPS'] == 'on' || env['HTTP_X_FORWARDED_PROTO'] == 'https'
      @app.call(env)
    else
      req = Rack::Request.new(env)
      [301, { "Location" =&gt; req.url.gsub(/^http:/, "https:") }, []]
    end
  end
end

# config/application.rb
config.autoload_paths += %W( #{ config.root }/lib/middleware )

# config/environments/production.rb
config.middleware.use "ForceSSL"
</pre>

## Mark Rails session cookies as &#8220;secure&#8221;

We need to mark cookies as secure in order to prevent the browser from sending them in the clear if a user goes to &#8220;http&#8221; instead of &#8220;https&#8221; on your app. Your server will redirect the browser to the &#8220;https&#8221; URL, however any cookies that are not marked as secure will be transmitted in the clear with the original http request and are thus vulnerable.

<pre class="brush: ruby; title: ; notranslate" title=""># config/initializers/session_store.rb
TiroApp::Application.config.session_store(
  :cookie_store,
  :key =&gt; '_tiro_app_session',
  :secure =&gt; Rails.env.production?, # Only send cookie over SSL when in production mode
  :http_only =&gt; true # Don't allow Javascript to access the cookie (mitigates cookie-based XSS exploits)
)

# config/initializers/secret_token.rb
TiroApp::Application.config.secret_token = 'put a really long secret string here...'
</pre>

Depending on what authentication library you use, your next steps might vary.

## Mark Authlogic persistence cookies as &#8220;secure&#8221;

I use authlogic as authentication library for tiroapp.com. authlogic doesn&#8217;t mark its session persistence cookies as secure by default. So we will monkey patch it to do so:

<pre class="brush: ruby; title: ; notranslate" title=""># /config/initializers/authlogic_secure_cookie_patch.rb
#
# This patch makes authlogic's persistence cookie
# secure to prevent HTTP session hijacking
module Authlogic
  module Session
    module Cookies
      module InstanceMethods

      private

        def save_cookie
          controller.cookies[cookie_key] = {
            :value =&gt; "#{record.persistence_token}::#{record.send(record.class.primary_key)}",
            :expires =&gt; remember_me_until,
            :domain =&gt; controller.cookie_domain,
            :secure =&gt; Rails.env.production?, # Only send cookie over SSL when in production mode
            :httponly =&gt; true
          }
        end
      end
    end
  end
end
</pre>

Whew, now your app is really secure!

Let me know if I left any security holes open (in regard to session persistence and communications security). Please note that in order to have your app really secure, there are a lot more things to consider. But that&#8217;s another blog post or two ;-).

## Credits

  * [Eric Butler][1] for raising awareness for this issue with his Firesheep plugin
  * [jmtame][3] for a great walk through through the certificate process. I varied it in a number of places to adapt it to my needs
  * [Simone Carletti][4] for the ForceSSL rack middleware
  * [bioneuralnet][5] for the secure rails session cookie configuration

 [1]: http://codebutler.com/firesheep
 [2]: http://tiroapp.com
 [3]: http://jmtame.posterous.com/ssl-certificates-with-godaddy-and-heroku
 [4]: http://stackoverflow.com/questions/3861772/force-ssl-using-ssl-requirement-in-rails-app/3862679#3862679
 [5]: http://stackoverflow.com/questions/4147387/how-to-secure-a-rails-app-against-firesheep/4148651#4148651
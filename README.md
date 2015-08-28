cluesque fork
=============================================

This fork (living on both github and bitbucket) fixes the `debug` parameter, plumbing it through to the excon http layer below.

A copy lives on github for convenience / reliability of deploy.

Overview
=============================================
A Ruby API client for [v2 of the MailChimp API](http://apidocs.mailchimp.com/api/2.0/). Please note that we generate this client/wrapper, so while we're happy to look at any pull requests, ultimately we can't technically accept them. We will, however comment on any additions or changes made due to them before closing them.


###Usage
This can be installed via our [gem](https://rubygems.org/gems/mailchimp-api) using:
```
$ gem install mailchimp-api
```
Then you'll need to `require 'mailchimp'` in your code.

Alternatively, if you're using a recent version of Bundler, you can add the following to your Gemfile:
```
gem 'mailchimp-api', require: 'mailchimp'
```
Then `bundle install` and you're all set.


---

A basic example app to help kickstart you which - will also automatically install this package - is over in a public [github](https://github.com/mailchimp/mcapi2-ruby-examples) repo

Make the following constants, in your environment file or where ever you are mentioning the constants.


    MAILCHIMP-API-KEY = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx-xxx"


You can have a look [here][4] (mailchimp documentation), how you can get the API-KEY.

Now there are multiple things that you can do from mailchimp-api. I am mentioning the basic's that used commonly.

Now create the List at mailchimp and [get the listId][5].

Add it in your constants, where you mentioned the API-KEY.


MAILCHIMP-LIST-ID = "xxxxxxxxxx"

Now the time is to get to know how to use [mailchimp-api gem][6]

UseCases
--------

Following line will create the object of mailchimp, we will access the mailchimp-api method using this object.

    mailchimp = Mailchimp::API.new(MAILCHIMP-API-KEY)


Use cases that we will be using are listed below.

 1. **Fetch All Members**
 2. **Get Members Info** 
 3. **Get All Lists**
 4. **Subscribe Single User**
 5. **Subscribe Members in Batch**
 6. **Get All Campaigns**

Fetch All Members
----------------

    mailchimp.lists.members(MAILCHIMP-LIST-ID)['data']


Get Members Info
----------------


    emails = [{
                "email" => {
                             "email" => "awais@testing.com",
                             "euid"  => "275",
                             "leid"  => "27546289917a6"
                           }
             }]

    mc.lists.member_info(MAILCHIMP-LIST-ID, emails)

for further information of this method you can have look [here][7]

Get All Lists
----------------

      lists = mailchimp.lists.list
      list  = lists['data']


Subscribe Single User
---------------------

    mailchimp.lists.subscribe(MAILCHIMP-LIST-ID, 
                       { "email" => "awais545@gmail.com",
                         "euid" => "123",
                         "leid" => "123123"
                       })

for further information of this method you can have look [here][8]

Batch Subscribe
----------------

Example for this method is not even given in [mailchimp-api sample project][9]. 

In order to do bulk upload of subscribers. MailchimpApi provides you a method using which you can simply pass the hash of data and in single call all of your subscribers will be added at mailchimp.

Hash would be created in following format


    subscribers = [{ "EMAIL" => { "email" => awais545@gmail.com,
                     "EUID" => "123",
                     "LEID" => "123123"
                   },

                   :EMAIL_TYPE => 'html',
                   :merge_vars => { "FIRSTNAME" => "Muhammad Awais",
                                    "LASTNAME"  => "Muhammad Ilyas",
                                    "STATUS"    => "Subscribed",
                                    "LBOOKCNTRY" => "United States"
                                  }
                  }]

Now you can simply call the following method and it will do bulk upload at mailchimp.

    mailchimp.lists.batch_subscribe(MAILCHIMP-LIST-ID, subscribers, false, true, false)

You can have look [here][10](mailchimp api documentation) or [here][11] (ruby-docs)to get further information about batch-subscribe.

Get All Campaigns
-----------------

    mailchimp.campaigns.list

You can also filtered the campaigns on there status as follow

    mailchimp.campaigns.list({:status=>'sent'})

NOTE
------------

**This is documentation is for mailchimp-api 2.0.4**

Contributing
------------

 1. Fork it
 2. Create your feature branch (git checkout -b my-new-feature)
 3. Commit your changes (git commit -am 'Added some feature')
 4. Push to the branch (git push origin my-new-feature)
 5. Create new Pull Request

  [4]: http://kb.mailchimp.com/article/where-can-i-find-my-api-key
  [5]: http://kb.mailchimp.com/article/how-can-i-find-my-list-id/
  [6]: https://rubygems.org/gems/mailchimp-api
  [7]: http://rubydoc.info/gems/mailchimp-api/2.0.4/Mailchimp/Lists#member_info-instance_method
  [8]: http://rubydoc.info/gems/mailchimp-api/2.0.4/Mailchimp/Lists#subscribe-instance_method
  [9]: https://github.com/mailchimp/mcapi2-ruby-examples
  [10]: http://apidocs.mailchimp.com/api/2.0/lists/batch-subscribe.php
  [11]: http://rubydoc.info/gems/mailchimp-api/2.0.4/Mailchimp/Lists#batch_subscribe-instance_method
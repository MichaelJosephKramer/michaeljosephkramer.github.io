---
layout: post
title: Escape Regular Expressions in Rails Migrations
---

Rails migrations are great way to manage repeatable database changes across environments, but they're not without their complications. Sometimes a regular old SQL statement is all you really need to do the job, but mixing the two sometimes doesn't work so well.

Lets say, hypothetically, that some bad data ended up in your database -- although this could never happen in the real world. You decide to strip out all the non-numeric characters. After spending way too much time scouring the Postgresql docs, you finally come up with this gem:

~~~sql
UPDATE users
SET phone = regexp_replace(phone, '[^\d]', '', 'g')
WHERE phone IS NOT NULL;
~~~

As you can imagine, that statement updates the phone number with a phone number that has only digits, with the regex replacing all non-numeric charaters with '', and doing so globally across the string. Running it in the Postgresql console works perfectly and all is right with the world.

In order to run it across all envrionments, you put it in a Rails migration because you're a good developer. It looks something like this:

~~~ruby
class CleanPhoneNumbersInUsers < ActiveRecord::Migration
  def up
    execute <<-SQL
      UPDATE users
      SET phone = regexp_replace(phone, '[^\d]', '', 'g')
      WHERE phone IS NOT NULL;
    SQL
  end

  def down
    raise ActiveRecord::IrreversibleMigration
  end
end
~~~

To your horror, when you run this migration, all the numbers disappear. Whaa?

It turns out the regular wasn't escaped enough, and didn't make it all the way through to postgresql:

{% highlight sql %}

SET phone = regexp_replace(phone, '[^\\d]', '', 'g')

{% endhighlight %}

So, in the process of escaping the regular expression, in the SQL, in the Ruby migration, the `\d` didn't do what it was supposed to do, but if you add the extra `\`, then you'll be back in business. 

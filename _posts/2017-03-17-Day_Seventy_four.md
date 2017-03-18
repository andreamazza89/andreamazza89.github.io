---
layout: post
title: 8th Light apprenticeship - Day Seventy-four
categories: 8thLight apprenticeship
---

This is a joint post by Andrea and EA (AndrEA) about how we solved the issue of null acceptance criteria in Artisan 2. It was a relatively simple problem that had a few options. 

Symptom

When clicking on a story from the storyboard view, if the acceptance criteria section had a null value in the database, the story would fail to open. To the user, it would just not open, if you probed a little deeper and opened the console, you would find an error telling you that ‘replace’ (function of string) could not be called on null.

Further digging revealed the replace call was being made in part of the client side javascript that rendered links to other stories in the format #{story_number}.

Problem

The root cause of this problem is to do with the fact that an older version of the same application exists. This has its own existing data, which contains some stories with the acceptance criteria field set to null. Migrating data from the old version of the application to the new one introduced these ‘rogue’ stories that caused the symptom described above. 

Solution

We we were in agreement that the solution would be with the underlying database.
In any case, we altered the database schema to not accept NULL values for the acceptance criteria field. We came up with a few plans of action to handle this change we made.

We needed to convert any NULL value for acceptance_criteria into an empty string. This could be done in a number of ways:

Manually enter SQL instructions to convert from NULL to empty string once the migration is done.
Write a rake task to do the same as above automatically.
Alter the script that migrates data from the old to the new database to convert the NULL values before writing them into the new database.
Write a (rails-y) migration for the new database to convert any NULL values left after importing the old data.

We chose option 4. We wrote two migrations. The first will select any of the stories with a null value and change it to an empty string. The second changes the database schema to enforce no null values on the acceptance criteria column. 
Why did we pick this solution?

We didn’t pick manual SQL because we believed the task could be more automated. 
We didn’t pick a rake task because we wanted it to be more enforced than a rake task and the setup was already there for migration scripts to be run.
Our solution fit more with the conventions of the codebase. 
Changing the migration script from old to new would not solve the problem of null values being allowed.
Migration was easy and simple to write.

Did it work?

Aside from missing a one line addition to the ORM models which broke the build - it worked and we are waiting for feedback. 

Conclusion

EA - I learned from this that it is important to consider all options.

Andrea - It is interesting to see how much there is to learn and consider from such a small problem. I learned a lot!

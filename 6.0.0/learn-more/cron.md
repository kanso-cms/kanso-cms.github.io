# CRON Support

- [Database Maintenance](#database-maintenance)
- [Email Queue](#email-queue)

--------------------------------------------------------

By default, Kanso does not require any `CRON` jobs to function correctly. However, if you are using the `Ecommerce`, `CRM` or Email Queue components, you should have some basic `CRON` jobs setup.

Kanso comes built in with 2 default routes for basic `CRON` jobs.

--------------------------------------------------------

### Database Maintenance

The `CRM` component logs every visit anyone makes to your website. For high traffic websites, this can create a very large database which eventually can effect performance. A `CRON` job route has already been setup for you which sanitizes the database and removes older entries.

To use the database maintenance job you will need to setup a scheduled task on your server to fetch the following URL:

```
https://example.com/cron-database-maintenance/?key=YOUR_APPLICATION_SECRET
```
Where `YOUR_APPLICATION_SECRET` is your secret found under `secret` in the `app/configurations/application.php` file.


--------------------------------------------------------

### Email Queue

If Kanso's email queue is enabled, you will need to process the queue via a cron job. Typicially this can be done by setting up a scheduled task on your server to fetch the following URL every 15 minutes:

```
https://example.com/cron-email-queue/?key=YOUR_APPLICATION_SECRET
```
Where `YOUR_APPLICATION_SECRET` is your secret found under `secret` in the `app/configurations/application.php` file.
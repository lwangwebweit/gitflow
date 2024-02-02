---
description: Setup email for local environment
---

# Email

To test email on your local environment you need to have this configuration&#x20;

* make sure you have this  value in .env file MAILER\_URL=[smtp://mailhog:1025](smtp://mailhog:1025)
*   go to email setting in this page [https://interpneu.dev.webweit.de/admin#/sw/settings/mailer/index](https://interpneu.dev.webweit.de/admin#/sw/settings/mailer/index)\
    and change **Preferred email agent** to **Use environment's configuration**\


    <figure><img src=".gitbook/assets/Screenshot 2023-11-08 at 15.10.18 (1).png" alt=""><figcaption></figcaption></figure>
*   then you can access the mailhog from this link [ http://localhost:8025/](http://localhost:8025/) \
    and the page will looks like this\




    <figure><img src=".gitbook/assets/Screenshot 2023-11-08 at 15.10.56.png" alt=""><figcaption></figcaption></figure>

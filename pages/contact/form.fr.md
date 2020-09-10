---
title: Contact
visible: false
form:
    name: contact
    fields:
        name:
            label: nom
            placeholder: 'Votre nom'
            autocomplete: 'on'
            type: text
            validate:
                required: true
        email:
            label: email
            placeholder: 'Votre adrese email'
            type: email
            validate:
                required: true
        message:
            label: message
            placeholder: 'Saisissez votre message'
            type: textarea
            rows: 6
            validate:
                required: true
        g-recaptcha-response:
            label: Captcha
            type: captcha
            recaptcha_not_validated: 'Captcha non valide!'
    buttons:
        submit:
            type: submit
            value: Submit
        reset:
            type: reset
            value: Reset
    process:
        captcha: true
        save:
            fileprefix: contact-
            dateformat: Ymd-His-u
            extension: txt
            body: '{% include ''forms/data.txt.twig'' %}'
        email:
            subject: '[Site Contact Form] {{ form.value.name|e }}'
            body: '{% include ''forms/data.html.twig'' %}'
        message: 'Votre message à bien été envoyé&nbsp;!'
        display: message-envoye
---

&#x266F; noguarantee ^^

Pour toute question, n'hésitez pas à me contacter&nbsp;! Les articles présentés ici sont pour certains particulièrement datés et je n'utilise plus certains des logiciels ou packages LaTeX présentés. Je tâcherai de répondre du mieux que je pourrai le plus rapidement possible.
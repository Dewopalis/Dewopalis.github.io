---
layout: page
title: Contact
permalink: /contact/
---

{% if site.formspree.form_id and site.formspree.form_id != "" %}
## Contact form

<form action="https://formspree.io/f/{{ site.formspree.form_id | escape }}" method="POST" class="contact-form">
  <label for="contact-name">Name <span>(optional)</span></label>
  <input id="contact-name" name="name" type="text" autocomplete="name">

  <label for="contact-email">Your email</label>
  <input id="contact-email" name="_replyto" type="email" autocomplete="email" required>

  <label for="contact-message">Message</label>
  <textarea id="contact-message" name="message" rows="7" required></textarea>

  <label class="contact-form-honeypot" aria-hidden="true">Leave this field empty
    <input name="_gotcha" tabindex="-1" autocomplete="off">
  </label>

  <button type="submit">Send message</button>
</form>

<p>Formspree processes this message so it can be delivered to me. Please avoid
sending sensitive personal information.</p>
{% else %}
## Send a private message

I'm setting up a private contact form so you can get in touch without my email
address being published on this site. Check back soon.

To comment publicly, open a post and use its discussion area. Comments are
hosted on GitHub Discussions, and your GitHub username will be visible to
other readers.
{% endif %}

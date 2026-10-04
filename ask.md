---
layout: page
title: Ask a Question
permalink: /ask/
---
{% comment %}
To switch the anonymous form on: get a free access key at web3forms.com, paste it between the quotes below, save and commit.
Leave it empty and only the email option shows.
{% endcomment %}
{% assign w3key = "51d7bf24-0b4c-40c7-b087-2279f15408e3" %}

Do you have a question about religion, belief, science or how we know what we know? Ask it here. **You do not need to give a name or an email.** I read every question, and the best ones are answered on the [Questions and Answers](/questions/) page or become a full post.

{% if w3key != "" %}
<form id="ask-form" style="margin:1.2rem 0;">
<input type="hidden" name="access_key" value="{{ w3key }}">
<input type="hidden" name="subject" value="New question for Earth and Faith">
<input type="hidden" name="from_name" value="Anonymous reader">
<input type="checkbox" name="botcheck" style="display:none" tabindex="-1" autocomplete="off">
<label for="ask-q" style="display:block;font-weight:700;margin-bottom:0.4rem;">Your question</label>
<textarea id="ask-q" name="message" required minlength="10" maxlength="1500" rows="6" placeholder="Write your question here. Please do not include your name, phone number or other personal details." style="width:100%;box-sizing:border-box;padding:0.7rem 0.9rem;font:inherit;border:1px solid #c9d1cc;border-radius:10px;"></textarea>
<p id="ask-count" style="font-size:0.85rem;margin:0.3rem 0 0.8rem;opacity:0.7;">0 / 1500</p>
<button type="submit" id="ask-btn" style="padding:0.6rem 1.3rem;border:0;border-radius:10px;background:#1f6f5c;color:#fff;font:inherit;font-weight:700;cursor:pointer;">Send my question</button>
<p id="ask-msg" role="status" style="margin-top:0.8rem;font-weight:600;"></p>
</form>
<script>
(function () {
  var f = document.getElementById("ask-form"), q = document.getElementById("ask-q"), c = document.getElementById("ask-count"), m = document.getElementById("ask-msg"), b = document.getElementById("ask-btn");
  q.addEventListener("input", function () { c.textContent = q.value.length + " / 1500"; });
  f.addEventListener("submit", function (e) {
    e.preventDefault();
    b.disabled = true; m.textContent = "Sending...";
    var o = {}; new FormData(f).forEach(function (v, k) { o[k] = v; });
    fetch("https://api.web3forms.com/submit", { method: "POST", headers: { "Content-Type": "application/json", "Accept": "application/json" }, body: JSON.stringify(o) })
      .then(function (r) { return r.json(); })
      .then(function (j) {
        if (j && j.success) { m.textContent = "Thank you. Your question was sent. If I answer it, it will appear on the Questions page."; f.reset(); c.textContent = "0 / 1500"; }
        else { m.textContent = "Sorry, it could not be sent. Please try again later or use email."; }
        b.disabled = false;
      })
      .catch(function () { m.textContent = "Sorry, it could not be sent. Please check your internet and try again."; b.disabled = false; });
  });
})();
</script>
{% endif %}

## Before you send

- **Your question is sent without a name.** I do not ask for your name or email, and I do not see who you are. If you put a name or contact detail inside the question, I will remove it before anything is published.
- By sending it, you agree that I may publish your question and my answer on this site, without any name.
- I cannot answer every question, and answers may take some time. I cannot reply to you personally, because I do not know who you are.
- Please be respectful. Abusive, hateful or threatening messages will be deleted.
- I do not give medical, legal or personal advice.
- The form is run by an outside service, which may see your IP address when you send. I do not store it. See the [Privacy Policy](/privacy-policy/) for details.

## Want a personal reply?

If you would like me to answer you directly, email me at [contact@earthandfaith.online](mailto:contact@earthandfaith.online?subject=My%20question%20for%20Earth%20and%20Faith). Then I will know your email address, as explained in the [Privacy Policy](/privacy-policy/).

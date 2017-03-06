# Send mail templates  with mandrill

First of all you must have a mandrill account with the template that you what to send.

if the template has dynamic content you must use `mc:edit=` in order to inject content inside your html tags

```html
<div id="wrapper">
      <div mc:edit="token">
      </div>
</div>
```

## Using the the mandrill client

The mandrill client its very simple, you only need to specify some fields in a `mailOpts` objects, for example:
- `template_name` : The same name of your mandrill template.

- `template_content`: Its an array of objects with the content that you want to inject in your template throught the `mc:edit=`.

- `message.to`: In this field you put the user mail.

Then you call the `messages.sendTemplate()` function with your `mailOpts`, this its going to send the template and return a callback.

```js
import mandrill from 'mandrill-api/mandrill';
const mandrillClient = new mandrill.Mandrill(YOUR_KEY);

var mailOpts = {
  template_name: "Your Template name ",
  template_content: [
      {
          name: "token",
          content: "<p> dynamic data</p>"
      }
  ],
  message: {
      to: [{
              email: 'example@mandrill.com'
              type: "to"
          }],
      merge_language: "mailchimp"
  },
  async: false
};

mandrillClient.messages.sendTemplate(mailOpts,cb)
```

### Doc

[Mandrill doc: send template ](https://mandrillapp.com/api/docs/messages.JSON.html#method=send-template)

# Sending Email with Sendgrid

```
const sgMail = require('@sendgrid/mail');

process.env.DEBUG = 'dialogflow:debug';
process.env.SENDGRID_API_KEY = 'INSERT SENDGRID API KEY HERE', //Create NodeJS API Key in Sendgrid

	function sendEmail(agent) {
		sgMail.setApiKey(process.env.SENDGRID_API_KEY);
		const emailParam = agent.parameters.email;
		const msg = {
			to: emailParam,
			from: 'email.botcopytest@gmail.com',
			subject: 'Looks like we missed you. You probably caught us sleeping.',
			text: 'Hi, Keystroke (our bot) just sent us your email to follow up. How can we help you today?',
			//html: '',
		};
		sgMail.send(msg);
		agent.add('Got it. Thank you! I will make sure our team reaches out to you soon. You are free to continue to hold. Feel free to ask me any questions while you wait.');
	}

let intentMap = new Map();
intentMap.set('email_Capture', sendEmail);
agent.handleRequest(intentMap);
});
```
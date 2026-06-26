# Twilio Flex Auto-Translation

Botcopy can automatically translate messages between your widget users and your Twilio Flex agents. Incoming user messages are translated into your agents' language, and agent replies are translated back into each user's language. An optional glossary lets you enforce how specific terms (product names, brand terms, industry jargon) are translated.

Translation uses Google's [Cloud Translation API](https://cloud.google.com/translate/docs) running on the Google Cloud project connected to your bot.

> **Availability:** Auto-Translation is part of the Twilio Flex live chat integration. If you don't see the Auto-Translation section in the Portal, contact Botcopy to have it enabled for your organization.

## Before You Begin

You'll need:

1. A Twilio Flex integration already connected to your bot (credentials assigned on the bot). See [Twilio Flex Live Chat](livechat/twilio-flex.md) for setup.
2. A Google Cloud project connected to the bot, with the **Cloud Translation API** enabled. You can enable it from the [Cloud Translation API library page](https://console.cloud.google.com/apis/library/translate.googleapis.com).
3. The bot's Google Cloud service account granted the **Cloud Translation API User** role (`roles/cloudtranslate.user`) on that project. This role includes the `cloudtranslate.generalModels.predict` permission used to translate text.

## Enabling Translation

1. In the [Portal](https://portal.botcopy.com), open your bot and go to the **Integrations** page.
2. Expand the **Twilio Flex** section and assign your Flex credentials if you haven't already.
3. Under **Auto-Translation**, turn on **Enable Translation**.
4. Click **Test Translation API Access** (see [Validating Your Setup](#validating-your-setup)). You must get a successful result before you can save.
5. Click **Save**.

## Agent Language

The **Agent Language** dropdown sets the language your Twilio Flex agents read and write in. Widget messages are translated into this language for your agents, and agent replies are translated back into each user's detected language.

> Agent Language currently supports **English (`en`)**. Additional agent languages will be added over time. End users can still chat in any language Cloud Translation supports; only the agent-facing language is fixed.

## Supported User Languages

End-user languages are detected and translated automatically by Google Cloud Translation. For the full list of languages and codes, see Google's [Language support](https://cloud.google.com/translate/docs/languages) reference.

## Glossary (Optional)

A glossary enforces custom translations for specific terms so that, for example, your product names or branded phrases are always translated consistently (or left untranslated). Glossaries are optional. When no glossary is set, translation uses Google's default model.

### How it works

The glossary lives in **your own Google Cloud project**, not Botcopy's. You create and own the glossary, and you paste its resource name into the Portal. The project and location are read directly from that resource name, so the glossary can live in the same project as your bot or any project where your bot's service account has access.

### 1. Create the glossary in your Google Cloud project

Follow Google's guide to [create and manage glossaries](https://cloud.google.com/translate/docs/advanced/glossary). In short:

1. Prepare a glossary file (CSV, TSV, or TMX) mapping your source terms to their translations.
2. Upload the file to a Cloud Storage bucket in your project.
3. Create the glossary resource via the Cloud Translation API, referencing that file.

### 2. Grant the required permission

The bot's service account needs the **`cloudtranslate.glossaries.predict`** permission on the project that holds the glossary. This permission is included in the **Cloud Translation API User** role (`roles/cloudtranslate.user`). If the glossary lives in a different project than the bot, grant that role to the bot's service account on the glossary's project as well.

### 3. Find the resource name

Your glossary's resource name follows this format:

```
projects/PROJECT/locations/LOCATION/glossaries/GLOSSARY
```

For example: `projects/acme-prod/locations/us-central1/glossaries/support-terms`. You can find it in the response when you create the glossary, or by [listing glossaries](https://cloud.google.com/translate/docs/advanced/glossary#list-glossary) via the API.

### 4. Add it to the Portal

1. In the bot's **Auto-Translation** settings, paste the resource name into **Glossary ID (optional)**.
2. Click **Test Glossary** to confirm Botcopy can reach it (see below).
3. Click **Save**.

## Validating Your Setup

Two test buttons let you confirm everything works before saving:

- **Test Translation API Access** — verifies the bot's service account can reach the Cloud Translation API on the connected project. A successful result is required before you can save translation settings. If it fails, the error tells you what to fix (most commonly a missing `roles/cloudtranslate.user` role or the Cloud Translation API not being enabled).
- **Test Glossary** — verifies the glossary resource name is valid and reachable by the bot's service account. Run this after pasting a Glossary ID.

If you change any translation setting after testing, re-run the relevant test before saving.

## Privacy and Data Handling

When translation is enabled, the text of messages exchanged between your users and your Twilio Flex agents is sent to **Google's Cloud Translation API** for processing. This includes whatever your users type.

You are responsible for:

- Informing your end users that their messages may be processed by a third-party translation service.
- Meeting your own data-retention, privacy, and regulatory/compliance obligations.

Review Google's [Cloud Translation data usage](https://cloud.google.com/translate/data-usage) documentation to understand how Google handles the text it receives. Because translation runs on **your** Google Cloud project, the relationship and terms for that processing are between you and Google.

## Troubleshooting

- **"Test required" / can't save:** Enable translation, run **Test Translation API Access**, and get a successful result first.
- **Translation test fails with a permission error:** Confirm the Cloud Translation API is enabled and the bot's service account has `roles/cloudtranslate.user` on the connected project.
- **Glossary test fails:** Check the resource name format, confirm the glossary exists in the named project/location, and confirm the bot's service account has `cloudtranslate.glossaries.predict` (via `roles/cloudtranslate.user`) on that project.
- **Agents receive untranslated messages:** If translation can't run, Botcopy sends agent replies untranslated and may hold incoming messages until access is restored, so the conversation is never blocked. Re-run the tests to find the cause.

# Unique URL slug

This is a [custom element](https://kontent.ai/learn/docs/custom-elements) for [Kontent by Kentico](https://kontent.ai) that generates a URL slug based on a text field or a custom value. It also checks for uniqueness of such value and can generate a unique value if the same URL slug is found.

![Screenshot of custom element](customurlslug.gif)

# Getting Started

To start the development server run:
- `npm ci` - install dependencies
- `npm run dev` - start the development server usually on https://localhost:5173 (the port can be different if 5173 is already in use)
- The `npm run dev` generates a self-signed certificate for the development server.
  Open the URL in a browser and proceed to the website through the browser warning.
- Once you proceed, the browser will remember the exception and will not ask again. (Works in chromium-based browsers)
  Then the element will properly load in the `iframe` in Kontent.ai.
- Remember to check out the required configuration of the element in `src/customElement/config.ts` and provide the configuration in the content type.

## Setup

1. Deploy the code to a secure public host
   - See [deploying section](#deploying) for a really quick option
2. Follow the instructions in the [Kontent documentation](https://kontent.ai/learn/docs/custom-elements#a-3--displaying-a-custom-element-in-kentico-kontent) to add the element to a content model.
   - The `Hosted code URL` is where you deployed to in step 1
   - Pass the necessary parameters as directed in the [JSON Parameters configuration](#json-parameters) section of this readme.
3. Deploy your server repeater for Preview API calls (details below)

## Deploying

Netlify has made this easy. If you click the deploy button below, it will guide you through the process of deploying it to Netlify and leave you with a copy of the repository in your GitHub account as well.

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/kontent-ai-presales-engineering/custom-element-autofill)

## JSON Parameters

You need to specify the `repeater` URL, `generates_from` and `codename` parameters in order to make the element work. There are also optional `force_uniqueness` and `restricted_chars` parameters:

```json
{
  "nameElement": "title",
  "sourceElement": "title",
  "targetElement": "custom_slug",
  "previewApiKey": "your_preview_api_key",
  "managementApiKey": "your_management_api_key"
}
```
```
  - `repeater` -> A [URL of a service](https://kontent.ai/learn/develop/integrate/worked-examples/sensitive-data-in-custom-elements) you host on your server and that forwards JSON from Preview API based on "/items?elements.<url_slug_codename>[contains]=<url_slug_value>&depth=0" query
  - `generates_from` -> A codename of a source Text element you want your url slug to be generated from
  - `codename` -> A codename of your Custom URL Slug element (same codename across all types you want to check the uniqueness for)
  - `force_uniqueness` -> A true/false value that generates extra postfix to the url slug value in case the url slug is not unique
  - `restricted_chars` -> A set of characters (RegEx) you want the url slug to be restricted to

## Security

Since custom element code needs to be publicly accessible, you can't call Preview API directly due to Preview token exposure. You need to create your own server repeater that checks the origin, asks Preview API with the token and returns data back.

## Disclamer

With this custom element you can't use `URLslug` macro in Preview urls. Please use `ItemId` or `Codename` macros instead.


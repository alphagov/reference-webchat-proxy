# Webchat Proxy - Reference Implementation

This is a reference implementation of an API which departments who want to integrate webchat or virtual assistants on GOV.UK must expose.

## Motivation

Most engagement products require the addition of "tags" to pages that they wish to add webchat or virtual assistants to. These are essentially third-party scripts which are loaded on to that page.

We do not include third-party JavaScript on GOV.UK (except for Google Analytics). Doing so essentially provides that third party with complete control over the user's experience of GOV.UK, which has significant security and usability concerns.

The API documented here is the standard to which we expect all integrations to adhere.

The code in this repository is a reference implementation example of how a department or service could normalise a Webchat implementation to the GOV.UK Webchat Component needs. This particular implementation is for eGain, but the external API is applicable in all cases.

This proxy will give departments flexibility over implementing caching or changing internals of their endpoint, without GOV.UK's input, while maintaining the security of GOV.UK.

## Webchat Indentifier Mappings

Currently we keep a hardcoded mapping of URLs to their respective Webchat APIs. In the future we plan to allow these to be set in the Publishing Admin interface, but currently these will need to be handled in collaboration with GOV.UK.

## API Description

Anything other than a 200 response is a failure mode, JSON is returned, and CORS headers are set so this can be accessed by a browser.

---
### GET `/availability/:id`

`:id` should be the unique Webchat 'room' identifier

This endpoint is for checking the availability of a Webchat instance.

#### Responses

##### Webchat has a technical error
Any response that does not conform to an available response type will render a technical error,
as will a non 200 type response.

```
  {
    status: "failure",
    response: "error-message-related-to-the-techincal-error"
  }

```
##### Webchat is available
```
  response code: 200
  {
    status: "success",
    response: "AVAILABLE"
  }
```
##### WebChat is unavailable
```
  response code: 200
  {
    status: "success",
    response: "UNAVAILABLE"
  }
```
##### WebChat is busy
```
  response code: 200
  {
    status: "success",
    response: "BUSY"
  }
```
---

### GET `/open/:id`

`:id` should be the unique Webchat room identifier

This URL is used by the Webchat Component to open the correct Webchat instance. The Webchat component should be able to call `window.open` on this endpoint and open a Webchat instance in a new window.

In our reference example, we redirect to the underlying eGain API endpoint but this would change depending on your Webchat provider.

## This projects' dependencies

 - ruby (2.1>)
 - bundler

## How to run this project

```
git clone git@github.com:alphagov/reference-webchat-proxy.git
cd reference-webchat-proxy
bundle install
ruby app.rb
```

### Further documentation

The JavaScript and HTML side of this API is documented here in [the public code for GOV.UK's frontend](https://github.com/alphagov/government-frontend/blob/master/docs/webchat.md).

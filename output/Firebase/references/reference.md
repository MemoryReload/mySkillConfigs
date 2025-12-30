# Firebase - Reference

**Pages:** 96

---

## Prepare for Apple's App Store data disclosure requirements Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/ios/app-store-data-collection

**Contents:**
- Prepare for Apple's App Store data disclosure requirements Stay organized with collections Save and categorize content based on your preferences.
- Firebase user agent
- FirebaseCore
- GoogleUtilities
- GoogleDataTransport
  - Always collected
- FirebaseABTesting
- FirebaseAILogic
  - Always collected
  - Collected by default

Apple requires developers publishing apps on the App Store to disclose certain information regarding their apps’ data use.

This document contains Firebase Apple platform library behaviors that could require disclosure according to Apple's guidelines. When installing Firebase, take note of the build targets installed into your app by your dependency manager of choice. For each target that your dependency manager lists, review the corresponding section of this document to determine what data collection you must disclose. The number of Firebase build targets you have installed may be greater than the number you expected since some Firebase build targets have transient dependencies on others.

If you are using any optional product features that involve additional data or participating in any tests of new product features that involve additional data, be sure to check if those features or tests require additional data disclosures.

To ensure your app's disclosures are accurate, we recommend that you always use the latest version of each Firebase SDK.

The Firebase user agent is a bundle of information collected from most Firebase SDKs and includes the following: device, OS, app bundle ID, and developer platform. The user agent is never linked to a user or device identifier and is used by the Firebase team to determine platform and version adoption in order to better inform Firebase feature decisions.

Includes networking utilities which may be used by other SDKs to collect data.

A/B Testing does not collect data.

The Firebase A/B Testing SDK sets and uses Google Analytics user properties in order to specify membership in experiment groups for Firebase Remote Config and Firebase In-App Messaging.

Firebase AI Logic was formerly called "Vertex AI in Firebase" with the library FirebaseVertexAI. Also, Firebase AI Logic formerly had the library FirebaseAI.

Google Analytics data collection information can be found in this support article.

The App Distribution SDK is intended for beta testing usage only. Do not include the App Distribution SDK in your application when submitting to the App Store.

If data collection is enabled:

If Dynamic Links is used together with Google Analytics:

If Cloud Messaging is used together with Google Analytics:

If Remote Config is used together with Google Analytics:

If Remote Config personalization is used:

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/Candidate

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Candidate
  - content
    - Declaration
  - safetyRatings
    - Declaration
  - finishReason
    - Declaration
  - citationMetadata
    - Declaration

A struct representing a possible reply to a content generation prompt. Each content generation prompt may produce multiple candidate responses.

The response’s content.

The safety rating of the response content.

The reason the model stopped generating content, if it exists; for example, if the model generated a predefined stop sequence.

Cited works in the model’s response content, if it exists.

Metadata related to the URLContext tool.

Initializer for SwiftUI previews or tests.

Initializes a response from a decoder. Used for decoding server responses; not for public use.

---

## FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebasecore/api/reference/Classes/Timestamp

**Contents:**
- FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Timestamp
  - init(seconds:nanoseconds:)
    - Declaration
    - Parameters
  - +timestampWithSeconds:nanoseconds:
    - Parameters
  - init(date:)
    - Declaration
  - init()

A Timestamp represents a point in time independent of any time zone or calendar, represented as seconds and fractions of seconds at nanosecond resolution in UTC Epoch time. It is encoded using the Proleptic Gregorian Calendar which extends the Gregorian calendar backwards to year one. It is encoded assuming all minutes are 60 seconds long, i.e. leap seconds are “smeared” so that no leap second table is needed for interpretation. Range is from 0001-01-01T00:00:00Z to 9999-12-31T23:59:59.999999999Z. By restricting to that range, we ensure that we can convert to and from RFC 3339 date strings.

Creates a new timestamp.

the number of seconds since epoch.

the number of nanoseconds after the seconds.

Creates a new timestamp.

the number of seconds since epoch.

the number of nanoseconds after the seconds.

Creates a new timestamp from the given date.

Creates a new timestamp with the current date / time.

Returns a new Date corresponding to this timestamp. This may lose precision.

Returns the result of comparing the receiver with another timestamp.

the other timestamp to compare.

orderedAscending if other is chronologically following self, orderedDescending if other is chronologically preceding self, orderedSame otherwise.

Represents seconds of UTC time since Unix epoch 1970-01-01T00:00:00Z. Must be from 0001-01-01T00:00:00Z to 9999-12-31T23:59:59Z inclusive.

Non-negative fractions of a second at nanosecond resolution. Negative second values with fractions must still have non-negative nanos values that count forward in time. Must be from 0 to 999,999,999 inclusive.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes/FirebaseAI

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- FirebaseAI
- Public APIs
  - firebaseAI(app:backend:useLimitedUseAppCheckTokens:)
    - Declaration
    - Parameters
    - Return Value
  - generativeModel(modelName:generationConfig:safetySettings:tools:toolConfig:systemInstruction:requestOptions:)
    - Declaration
    - Parameters

The Firebase AI SDK provides access to Gemini models directly from your app.

Creates an instance of FirebaseAI.

A custom FirebaseApp used for initialization; if not specified, uses the default FirebaseApp.

The backend API for the Firebase AI SDK; if not specified, uses the default googleAI() (Gemini Developer API).

When sending tokens to the backend, this option enables the usage of App Check’s limited-use tokens instead of the standard cached tokens. Learn more about limited-use tokens, including their nuances, when to use them, and best practices for integrating them into your app.

A FirebaseAI instance, configured with the custom FirebaseApp.

Initializes a generative model with the given parameters.

Refer to Gemini models for guidance on choosing an appropriate model for your use case.

The name of the model to use; see available model names for a list of supported model names.

The content generation parameters your model should use.

A value describing what types of harmful content your model should allow.

A list of Tool objects that the model may use to generate the next response.

Tool configuration for any Tool specified in the request.

Instructions that direct the model to behave a certain way; currently only text content is supported.

Configuration parameters for sending requests to the backend.

Initializes an ImagenModel with the given parameters.

Refer to Imagen models for guidance on choosing an appropriate model for your use case.

The name of the Imagen 3 model to use.

Configuration options for generating images with Imagen.

Settings describing what types of potentially harmful content your model should allow.

Configuration parameters for sending requests to the backend.

Initializes a new TemplateGenerativeModel.

A new TemplateGenerativeModel instance.

Initializes a new TemplateImagenModel.

A new TemplateImagenModel instance.

[Public Preview] Initializes a LiveGenerativeModel with the given parameters.

Using the Firebase AI Logic SDKs with the Gemini Live API is in Public Preview, which means that the feature is not subject to any SLA or deprecation policy and could change in backwards-incompatible ways.

The name of the model to use.

The content generation parameters your model should use.

A list of Tool objects that the model may use to generate the next response.

Tool configuration for any Tool specified in the request.

Instructions that direct the model to behave a certain way; currently only text content is supported.

Configuration parameters for sending requests to the backend.

---

## FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebasecore/api/reference/Functions

**Contents:**
- FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Functions
  - FirebaseVersion()
    - Declaration

The following functions are available globally.

Returns the current version of Firebase.

---

## FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/ios

**Contents:**
- FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Classes
  - FirebaseApp
    - Declaration
  - FirebaseConfiguration
    - Declaration
  - FirebaseOptions
    - Declaration
  - Timestamp
    - Declaration

The following classes are available globally.

The entry point of Firebase SDKs.

Initialize and configure FirebaseApp using FirebaseApp.configure() or other customized ways as shown below.

The logging system has two modes: default mode and debug mode. In default mode, only logs with log level Notice, Warning and Error will be sent to device. In debug mode, all logs will be sent to device. The log levels that Firebase uses are consistent with the ASL log levels.

Enable debug mode by passing the -FIRDebugEnabled argument to the application. You can add this argument in the application’s Xcode scheme. When debug mode is enabled via -FIRDebugEnabled, further executions of the application will also be in debug mode. In order to return to default mode, you must explicitly disable the debug mode with the application argument -FIRDebugDisabled.

It is also possible to change the default logging level in code by calling FirebaseConfiguration.shared.setLoggerLevel(_:) with the desired level.

This interface provides global level properties that the developer can tweak.

This class provides constant fields of Google APIs.

A Timestamp represents a point in time independent of any time zone or calendar, represented as seconds and fractions of seconds at nanosecond resolution in UTC Epoch time. It is encoded using the Proleptic Gregorian Calendar which extends the Gregorian calendar backwards to year one. It is encoded assuming all minutes are 60 seconds long, i.e. leap seconds are “smeared” so that no leap second table is needed for interpretation. Range is from 0001-01-01T00:00:00Z to 9999-12-31T23:59:59.999999999Z. By restricting to that range, we ensure that we can convert to and from RFC 3339 date strings.

---

## Firebase API Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference

**Contents:**
- Firebase API Reference Stay organized with collections Save and categorize content based on your preferences.
  - Apple platforms
  - Android
  - Web
  - Flutter
  - Unity
  - C++
  - Admin
- Other reference documentation
- Index of all libraries

The API reference documentation provides detailed information for each of the classes and methods in the Firebase SDK. Choose your preferred platform from the list below.

View Swift & Obj-C reference docs

View Kotlin & Java reference docs

View JS client reference docs

View all Flutter packages

View Unity reference docs

View C++ reference docs

View Admin SDK reference docs

Find detailed information about app configuration settings, or Firebase related tools.

View an index of all supported platforms, frameworks, libraries, and tools.

---

## API Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/js

**Contents:**
- API Reference Stay organized with collections Save and categorize content based on your preferences.
- Packages

---

## Troubleshooting & FAQ for Apple platforms and Firebase Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/ios/troubleshooting-faq

**Contents:**
- Troubleshooting & FAQ for Apple platforms and Firebase Stay organized with collections Save and categorize content based on your preferences.
    - What versions of Xcode does Firebase support?
    - My app prompts the user for their password to access Keychain items on macOS. How do I fix this?
    - Why does Firebase require the Keychain Sharing capability on macOS?
    - In Xcode versions 13 and later, why can my UIKit apps not open some URLs I've registeredin my Info.plist?

This page offers tips and troubleshooting for Apple platform-specific issues that you might encounter when using Firebase.

Have other challenges or don't see your issue outlined below? Make sure to check out the main Firebase FAQ for more pan-Firebase or product-specific FAQ.

You can also check out the Firebase Apple platforms SDK GitHub repo for an up-to-date list of reported issues and troubleshooting. We encourage you to file your own Firebase Apple platforms SDK related issues there, too!

Firebase supports up to two major versions of Xcode, not including versions of Xcode that Apple no longer supports. For example, starting in March 2019, Apple required at least iOS 12 on all apps, meaning Xcode 9 support was dropped and Xcode 10 was the only major version supported.

Changes to support for specific minor or patch versions of Xcode (for example, 9.2.0 to 9.4.1) are determined based on the needs of the Firebase Apple platforms SDK and a survey of developer usage. These changes are reflected in the Firebase Apple platforms SDK release notes and on the Firebase Apple platforms SDK setup page.

To see the minimum Xcode version supported by the SDK, check the requirements listed in Add Firebase to your Apple project.

Firebase support for Beta releases of Xcode is available on a "best effort" basis. Developers can track and submit issues in the Firebase Apple platforms SDK repository on GitHub.

Upgrade your Firebase dependency to version 9.6.0 or higher and add the [Keychain Sharing capability](/docs/ios/troubleshooting-faq#macos-keychain-sharing) to your target.

Firebase SDKs use keychain to store information like the Firebase installation ID used for FCM. Without Keychain access, Firebase SDKs may not function correctly. The macOS keychain behaves differently than the iOS-style keychain that is used on other platforms (iOS, tvOS, macCatalyst, and watchOS).

On macOS, apps use a shared keychain that may be modified by other apps and processes. Unlike iOS, there is no sandboxed keychain that the app has implicit access to. So, when a Mac app interacts with the keychain, the system prompts the user for access since the Mac app may be modifying a keychain item that it did not create. To address this discrepancy, Firebase queries the keychain with the kSecUseDataProtectionKeychain key, which tells the app to query a keychain item that is part of a keychain access group (this is default behavior on other platforms). The Keychain Sharing capability is required because the app needs it to synthesize an access group that can be shared amongst its targets, thus giving permission for the app to freely access keychain items in the access group.

For more information, see Apple's Keychain documentation .

Apple introduced a limit of 50 LSApplicationQueriesSchemes entries in Info.plist files. In 2015, Apple introduced LSApplicationQueriesSchemes to limit the number of URL queries each app could make. With the release of Xcode 13, these limits are enforced, while in Xcode 12 and earlier there was no effective limit to the number of schemes.

Some Firebase products, like Firebase Authentication and Firebase Dynamic Links, require the use of custom URL schemes to redirect to your application. These URLs conform to a concise and consistent URL scheme that should not count significantly against the 50 link scheme limit.

Note that for apps that continue to register more than 50 LSApplicationQueriesSchemes, some schemes will be silently ignored. The app may be unable to execute certain deeplinks, depending on the order in which they are added.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes/LiveSession

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- LiveSession
  - responses
    - Declaration
  - sendFunctionResponses(_:)
    - Declaration
    - Parameters
  - sendAudioRealtime(_:)
    - Declaration
    - Parameters

A live WebSocket session, capable of streaming content to and from the model.

Messages are streamed through responses, and can be sent through either the dedicated realtime API function (such as sendAudioRealtime(_:) and sendTextRealtime(_:)), or through the incremental API (such as sendContent(_:turnComplete:)).

To create an instance of this class, see LiveGenerativeModel.

An asynchronous stream of messages from the server.

These messages from the incremental updates from the model, for the current conversation.

Response to a LiveServerToolCall received from the server.

This method is used both for the realtime API and the incremental API.

Client generated function results, matched to their respective FunctionCallPart by the functionId field.

Sends an audio input stream to the model, using the realtime API.

To learn more about audio formats, and the required state they should be provided in, see the docs on Supported audio formats.

Raw 16-bit PCM audio at 16Hz, used to update the model on the client’s conversation.

Sends a video frame to the model, using the realtime API.

Instead of raw video data, the model expects individual frames of the video, sent as images.

If your video has audio, send it separately through sendAudioRealtime(_:).

For better performance, frames can also be sent at a lower rate than the video; even as low as 1 frame per second.

Encoded image data extracted from a frame of the video, used to update the model on the client’s conversation.

The IANA standard MIME type of the video frame data (eg; images/png, images/jpegetc.,).

Sends a text input stream to the model, using the realtime API.

Text content to append to the current client’s conversation.

Incremental update of the current conversation.

The content is unconditionally appended to the conversation history and used as part of the prompt to the model to generate content.

Sending this message will also cause an interruption, if the server is actively generating content.

Content to append to the current conversation with the model.

Whether the server should start generating content with the currently accumulated prompt, or await additional messages before starting generation. By default, the server will await additional messages.

Incremental update of the current conversation.

The content is unconditionally appended to the conversation history and used as part of the prompt to the model to generate content.

Sending this message will also cause an interruption, if the server is actively generating content.

Content to append to the current conversation with the model (see PartsRepresentable for conforming types).

Whether the server should start generating content with the currently accumulated prompt, or await additional messages before starting generation. By default, the server will await additional messages.

Permanently stop the conversation with the model, and close the connection to the server

This method will be called automatically when the LiveSession is deinitialized, but this method can be called manually to explicitly end the session.

Attempting to receive content from a closed session will cause a LiveSessionUnexpectedClosureError error to be thrown.

---

## Add Firebase to your Apple project Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/ios/setup

**Contents:**
- Add Firebase to your Apple project Stay organized with collections Save and categorize content based on your preferences.
- Prerequisites

Install the following:

Make sure that your project meets these requirements:

Set up a physical Apple device or use a simulator to run your app.

Do you want to use Cloud Messaging?

Do you want to use Cloud Messaging?

For Cloud Messaging on Apple platforms, here are the prerequisites:

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/GenerateContentResponse

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- GenerateContentResponse
  - UsageMetadata
    - Declaration
  - candidates
    - Declaration
  - promptFeedback
    - Declaration
  - usageMetadata
    - Declaration

The model’s response to a generate content request.

Token usage metadata for processing the generate content request.

A list of candidate response content, ordered from best to worst.

A value containing the safety ratings for the response, or, if the request was blocked, a reason for blocking the request.

Token usage metadata for processing the generate content request.

The response’s content as text, if it exists.

A summary of the model’s thinking process, if available.

Returns function calls found in any Parts of the first candidate of the response, if any.

Returns inline data parts found in any Parts of the first candidate of the response, if any.

Initializer for SwiftUI previews or tests.

---

## RulesTestEnvironment interface Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/emulator-suite/rules-unit-testing/rules-unit-testing.rulestestenvironment

**Contents:**
- RulesTestEnvironment interface Stay organized with collections Save and categorize content based on your preferences.
- Properties
- Methods
- RulesTestEnvironment.emulators
- RulesTestEnvironment.projectId
- RulesTestEnvironment.authenticatedContext()
  - Parameters
  - Example
- RulesTestEnvironment.cleanup()
- RulesTestEnvironment.clearDatabase()

A readonly copy of the emulator config specified or discovered at test environment creation.

The project ID specified or discovered at test environment creation.

Create a RulesTestContext which behaves like an authenticated Firebase Auth user.

Requests created via the returned context will have a mock Firebase Auth token attached.

At the very end of your test code, call the cleanup function. Destroy all RulesTestContexts created in test environment and clean up the underlying resources, allowing a clean exit.

This method does not change the state in emulators in any way. To reset data between tests, see clearDatabase(), clearFirestore() and clearStorage().

Clear all data in the Realtime Database emulator namespace.

Clear data in the default Firestore database for projectId in the Firestore emulator.

Clear Storage files and metadata in the active bucket in the Storage emulator.

Create a RulesTestContext which behaves like client that is NOT logged in via Firebase Auth.

Requests created via the returned context will not have Firebase Auth tokens attached.

**Examples:**

Example 1 (typescript):
```typescript
export interface RulesTestEnvironment
```

Example 2 (json):
```json
readonly emulators: {
        database?: HostAndPort;
        firestore?: HostAndPort;
        storage?: HostAndPort;
    };
```

Example 3 (unknown):
```unknown
readonly projectId: string;
```

Example 4 (unknown):
```unknown
authenticatedContext(user_id: string, tokenOptions?: TokenOptions): RulesTestContext;
```

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes/LiveGenerativeModel

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- LiveGenerativeModel
  - connect()
    - Declaration
    - Return Value

A multimodal model (like Gemini) capable of real-time content generation based on various input types, supporting bidirectional streaming.

You can create a new session via connect().

Start a LiveSession with the server for bidirectional streaming.

A new LiveSession that you can use to stream messages to and from the server.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/ImagenAspectRatio

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- ImagenAspectRatio
  - square1x1
    - Declaration
  - portrait9x16
    - Declaration
  - landscape16x9
    - Declaration
  - portrait3x4
    - Declaration

An aspect ratio for images generated by Imagen.

To specify an aspect ratio for generated images, set aspectRatio in your ImagenGenerationConfig. See the Cloud documentation for more details and examples of the supported aspect ratios.

Square (1:1) aspect ratio.

Common uses for this aspect ratio include social media posts.

Portrait widescreen (9:16) aspect ratio.

This is the landscape16x9 aspect ratio rotated 90 degrees. This a relatively new aspect ratio that has been popularized by short form video apps (for example, YouTube shorts). Use this for tall objects with strong vertical orientations such as buildings, trees, waterfalls, or other similar objects.

Widescreen (16:9) aspect ratio.

This ratio has replaced landscape4x3 as the most common aspect ratio for TVs, monitors, and mobile phone screens (landscape). Use this aspect ratio when you want to capture more of the background (for example, scenic landscapes).

Portrait full screen (3:4) aspect ratio.

This is the landscape4x3 aspect ratio rotated 90 degrees. This lets to capture more of the scene vertically compared to the square1x1 aspect ratio.

Fullscreen (4:3) aspect ratio.

This aspect ratio is commonly used in media or film. It is also the dimensions of most old (non-widescreen) TVs and medium format cameras. It captures more of the scene horizontally (compared to square1x1), making it a preferred aspect ratio for photography.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Enums

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Enumerations
  - GenerateContentError
    - Declaration
  - JSONValue
    - Declaration

The following enumerations are available globally.

Errors that occur when generating content from a model.

Represents a value in one of JSON’s data types.

This may be decoded from, or encoded to, a google.protobuf.Value.

---

## FirebaseABTesting Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseabtesting/api/reference/Classes/LifecycleEvents

**Contents:**
- FirebaseABTesting Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- LifecycleEvents
  - setExperimentEventName
    - Declaration
  - activateExperimentEventName
    - Declaration
  - clearExperimentEventName
    - Declaration
  - timeoutExperimentEventName
    - Declaration

An Experiment Lifecycle Event Object that specifies the name of the experiment event to be logged by Firebase Analytics.

Event name for when an experiment is set. It defaults to SetExperimentEventName and can be overridden. If experiment payload has a valid string of this field, always use experiment payload.

Event name for when an experiment is activated. It defaults to ActivateExperimentEventName and can be overridden. If experiment payload has a valid string of this field, always use experiment payload.

Event name for when an experiment is cleared. It is default to ClearExperimentEventName and can be overridden. If experiment payload has a valid string of this field, always use experiment payload.

Event name for when an experiment is timeout from being STANDBY. It is default to TimeoutExperimentEventName and can be overridden. If experiment payload has a valid string of this field, always use experiment payload.

Event name when an experiment is expired when it reaches the end of its TTL. It is default to ExpireExperimentEventName and can be overridden. If experiment payload has a valid string of this field, always use experiment payload.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Extensions/UIImage

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- UIImage
  - partsValue
    - Declaration

Enables images to be representable as PartsRepresentable.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/ExecutableCodePart

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- ExecutableCodePart
  - Language
    - Declaration
  - language
    - Declaration
  - code
    - Declaration
  - isThought
    - Declaration

A part containing code that was executed by the model.

The language of the code in an ExecutableCodePart.

The language of the code.

The code that was executed.

---

## FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebasecore/api/reference/Classes

**Contents:**
- FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Classes
  - FirebaseApp
    - Declaration
  - FirebaseConfiguration
    - Declaration
  - FirebaseOptions
    - Declaration
  - Timestamp
    - Declaration

The following classes are available globally.

The entry point of Firebase SDKs.

Initialize and configure FirebaseApp using FirebaseApp.configure() or other customized ways as shown below.

The logging system has two modes: default mode and debug mode. In default mode, only logs with log level Notice, Warning and Error will be sent to device. In debug mode, all logs will be sent to device. The log levels that Firebase uses are consistent with the ASL log levels.

Enable debug mode by passing the -FIRDebugEnabled argument to the application. You can add this argument in the application’s Xcode scheme. When debug mode is enabled via -FIRDebugEnabled, further executions of the application will also be in debug mode. In order to return to default mode, you must explicitly disable the debug mode with the application argument -FIRDebugDisabled.

It is also possible to change the default logging level in code by calling FirebaseConfiguration.shared.setLoggerLevel(_:) with the desired level.

This interface provides global level properties that the developer can tweak.

This class provides constant fields of Google APIs.

A Timestamp represents a point in time independent of any time zone or calendar, represented as seconds and fractions of seconds at nanosecond resolution in UTC Epoch time. It is encoded using the Proleptic Gregorian Calendar which extends the Gregorian calendar backwards to year one. It is encoded assuming all minutes are 60 seconds long, i.e. leap seconds are “smeared” so that no leap second table is needed for interpretation. Range is from 0001-01-01T00:00:00Z to 9999-12-31T23:59:59.999999999Z. By restricting to that range, we ensure that we can convert to and from RFC 3339 date strings.

---

## FirebaseABTesting Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseabtesting/api/reference/Classes

**Contents:**
- FirebaseABTesting Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Classes
  - ExperimentController
    - Declaration
  - LifecycleEvents
    - Declaration

The following classes are available globally.

This class is for Firebase services to handle experiments updates to Firebase Analytics. Experiments can be set, cleared and updated through this controller.

An Experiment Lifecycle Event Object that specifies the name of the experiment event to be logged by Firebase Analytics.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/CodeExecutionResultPart

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- CodeExecutionResultPart
  - Outcome
    - Declaration
  - outcome
    - Declaration
  - output
    - Declaration
  - isThought
    - Declaration

The result of executing code.

The outcome of a code execution.

The outcome of the code execution.

The output of the code execution.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Protocols

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Protocols
  - PartsRepresentable
    - Declaration
  - Part
    - Declaration

The following protocols are available globally.

A protocol describing any data that could be serialized to model-interpretable input data, where the serialization process cannot fail with an error.

A discrete piece of data in a media format interpretable by an AI model.

Within a single value of Part, different data types may not mix.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Extensions

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Extensions
  - UIImage
    - Declaration
  - CGImage
    - Declaration
  - CIImage
    - Declaration
  - String
    - Declaration

The following extensions are available globally.

Enables images to be representable as PartsRepresentable.

Enables CGImages to be representable as model content.

Enables CIImages to be representable as model content.

Enables a String to be passed in as PartsRepresentable.

---

## Log Query Language for Emulator Suite UI Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/emulator-suite

**Contents:**
- Log Query Language for Emulator Suite UI Stay organized with collections Save and categorize content based on your preferences.
- Keywords
  - level
  - search
  - metadata
    - metadata.emulator.name
    - metadata.function.name
  - user

Firebase Local Emulator Suite provides a rich user interface that includes support for viewing emulator logs. You can filter logs in the Emulator Suite UI using the query syntax described on this page.

The logs query language supports exact comparisons and and operations. Other operations are not currently supported.

Quotes are generally optional, except when using spaces or newlines.

Note this query syntax is available in Emulator Suite UI only. Emulators output additional logs in the *-debug.log files in your project directory (e.g., firestore-debug.log).

Log level. One of warn, info, error.

Text to match in a fuzzy search. For example, search=abc returns logs with the text "abc".

Use the search keyword to combine fuzzy searches with other keyword searches using the and operator.

Query on a specific emulator or on a function name.

Query logs from a specified emulator. One of firestore, functions, database, pubsub, hosting, storage.

The function name as defined in user app code.

Any JSON data the user logged from in-app code, for example:

The above log output can be queried with user.hello.

**Examples:**

Example 1 (unknown):
```unknown
// Find only info logs.
level=info

//Find logs for the sayHelloWorld function
metadata.emulator.name=functions
metadata.function.name=sayHelloWorld

//Find any log mentioning "hello world"
hello world // turns into search="hello world" internally

//Return any Hosting POST requests
metadata.emulator.name=hosting
search=POST
```

Example 2 (css):
```css
console.log(JSON.stringify({hello: world}))
```

---

## RulesTestContext interface Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/emulator-suite/rules-unit-testing/rules-unit-testing.rulestestcontext

**Contents:**
- RulesTestContext interface Stay organized with collections Save and categorize content based on your preferences.
- Methods
- RulesTestContext.database()
  - Parameters
- RulesTestContext.firestore()
  - Parameters
- RulesTestContext.storage()
  - Parameters

Get a Database instance for this test context. The returned Firebase JS Client SDK instance can be used with the client SDK APIs (v9 modular or v9 compat).

firebase.database.Database

a Database instance configured to connect to the emulator. It never connects to production even if a production databaseURL is specified

Get a Firestore instance for this test context. The returned Firebase JS Client SDK instance can be used with the client SDK APIs (v9 modular or v9 compat).

firebase.firestore.Firestore

a Firestore instance configured to connect to the emulator

Get a FirebaseStorage instance for this test context. The returned Firebase JS Client SDK instance can be used with the client SDK APIs (v9 modular or v9 compat).

firebase.storage.Storage

a Storage instance configured to connect to the emulator

**Examples:**

Example 1 (typescript):
```typescript
export interface RulesTestContext
```

Example 2 (unknown):
```unknown
database(databaseURL?: string): firebase.database.Database;
```

Example 3 (unknown):
```unknown
firestore(settings?: firebase.firestore.Settings): firebase.firestore.Firestore;
```

Example 4 (unknown):
```unknown
storage(bucketUrl?: string): firebase.storage.Storage;
```

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/GroundingMetadata

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- GroundingMetadata
  - webSearchQueries
    - Declaration
  - groundingChunks
    - Declaration
  - groundingSupports
    - Declaration
  - searchEntryPoint
    - Declaration

Metadata returned to the client when grounding is enabled.

If using Grounding with Google Search, you are required to comply with the “Grounding with Google Search” usage requirements for your chosen API provider: Gemini Developer API or Vertex AI Gemini API (see Service Terms section within the Service Specific Terms).

A list of web search queries that the model performed to gather the grounding information. These can be used to allow users to explore the search results themselves.

A list of GroundingChunk structs. Each chunk represents a piece of retrieved content (e.g., from a web page) that the model used to ground its response.

A list of GroundingSupport structs. Each object details how specific segments of the model’s response are supported by the groundingChunks.

Google Search entry point for web searches. This contains an HTML/CSS snippet that must be embedded in an app to display a Google Search entry point for follow-up web searches related to the model’s “Grounded Response”.

A struct representing the Google Search entry point.

Represents a chunk of retrieved data that supports a claim in the model’s response. This is part of the grounding information provided when grounding is enabled.

A grounding chunk sourced from the web.

Provides information about how a specific segment of the model’s response is supported by the retrieved grounding chunks.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Protocols/PartsRepresentable

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- PartsRepresentable
  - partsValue
    - Declaration

A protocol describing any data that could be serialized to model-interpretable input data, where the serialization process cannot fail with an error.

---

## Link Firebase dependencies statically or dynamically Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/ios/link-firebase-static-dynamic

**Contents:**
- Link Firebase dependencies statically or dynamically Stay organized with collections Save and categorize content based on your preferences.
- Static linking
- Dynamic linking

Beginning with CocoaPods 1.9.0 and Firebase 7, you can choose whether your Firebase dependencies are built as static or dynamic frameworks. We recommend using static frameworks unless you require certain dynamic library behaviors.

Note that libraries developed outside of GitHub can only be linked statically even with CocoaPods 1.9.0 and later. Currently, this library list includes AdMob, Analytics, Firebase ML, and Performance Monitoring. All other distribution channels, including the zip file, Swift Package Manager, and Carthage provide statically linked libraries only.

This document assumes a working knowledge of dynamic and static linking on Apple platforms. If you're unfamiliar with these concepts, take a look at the following documentation:

Since this document is concerned with the types of library linkage and not the loading of non-executable resource bundles, the terms library and framework are used interchangeably.

Statically linked libraries are bundled into your application executable at build time. As a result, the object files in the static library will be present in your app when it launches and do not need to be resolved at app-launch time by the dynamic linker. Consequently, apps using static linking will launch faster. This comes at the expense of a slightly larger binary / app executable, although it should be noted that the larger executable size will be offset by the lack of bundled dynamic libraries.

You can force static linking of Firebase dependencies by explicitly specifying the linkage in your Podfile:

Dynamically linked libraries are stored in your app bundle separately from your app's main executable, and they must be loaded at app-launch time by the dynamic linker. Apple's frameworks are all linked dynamically to enable code-sharing between processes; similarly, you can use dynamic frameworks to share code between your app and extension targets. You cannot share dynamic frameworks between separate applications, even if they are both signed by the same developer.

If you want to use Firebase as a dependency of a dynamic framework target, you also need to link Firebase dynamically; otherwise you'll run into duplicate class definitions in your app's runtime. Dynamic linking is the default behavior with use_frameworks!, but you can still explicitly specify dynamic linkage in your Podfile:

Note that dynamic linking may increase your app's launch time, especially if your app has a lot of dependencies.

**Examples:**

Example 1 (typescript):
```typescript
# cocoapods >= 1.9.0
use_frameworks! :linkage => :static
```

Example 2 (typescript):
```typescript
# cocoapods >= 1.9.0
use_frameworks! :linkage => :dynamic
```

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/Backend

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Backend
- Public API
  - vertexAI(location:)
    - Declaration
    - Parameters
  - googleAI()
    - Declaration

Represents available backend APIs for the Firebase AI SDK.

Initializes a Backend configured for the Gemini API in Vertex AI.

The region identifier, defaulting to us-central1; see Vertex AI locations for a list of supported locations.

Initializes a Backend configured for the Google Developer API.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/ContentModality

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- ContentModality
  - text
    - Declaration
  - image
    - Declaration
  - video
    - Declaration
  - audio
    - Declaration

Content part modality.

Returns the raw string representation of the ContentModality value.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/ImagenGenerationConfig

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- ImagenGenerationConfig
  - negativePrompt
    - Declaration
  - numberOfImages
    - Declaration
  - aspectRatio
    - Declaration
  - imageFormat
    - Declaration

Configuration options for generating images with Imagen.

See Parameters for Imagen models to learn about parameters available for use with Imagen models, including how to configure them.

Specifies elements to exclude from the generated image.

Defaults to nil, which disables negative prompting. Use a comma-separated list to describe unwanted elements or characteristics. See the Cloud documentation for more details.

Support for negative prompts depends on the Imagen model.

The number of image samples to generate; defaults to 1 if not specified.

The number of sample images that may be generated in each request depends on the model (typically up to 4); see the sampleCount documentation for more details.

The aspect ratio of generated images.

Defaults to to square, 1:1. Supported aspect ratios depend on the model; see ImagenAspectRatio for more details.

The image format of generated images.

Defaults to PNG. See ImagenImageFormat for more details.

Whether to add an invisible watermark to generated images.

If true, an invisible SynthID watermark is embedded in generated images to indicate that they are AI generated; false disables watermarking.

The default value depends on the model; see the addWatermark documentation for model-specific details.

Initializes configuration options for generating images with Imagen.

Specifies elements to exclude from the generated image; disabled if not specified. See negativePrompt.

The number of image samples to generate; defaults to 1 if not specified. See numberOfImages.

The aspect ratio of generated images; defaults to to square, 1:1. See aspectRatio.

The image format of generated images; defaults to PNG. See imageFormat.

Whether to add an invisible watermark to generated images; the default value depends on the model. See addWatermark.

---

## rules-unit-testing package Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/emulator-suite/rules-unit-testing/rules-unit-testing

**Contents:**
- rules-unit-testing package Stay organized with collections Save and categorize content based on your preferences.
- Functions
- Interfaces
- Type Aliases
- assertFails()
  - Parameters
  - Example
- assertSucceeds()
  - Parameters
  - Example

Assert the promise to be rejected with a "permission denied" error.

Useful to assert a certain request to be denied by Security Rules. See example below. This function recognizes permission-denied errors from Database, Firestore, and Storage JS SDKs.

a Promise that is fulfilled if pr is rejected with "permission denied". If pr is rejected with any other error or resolved, the returned promise rejects.

Assert the promise to be successful.

This is a no-op function returning the passed promise as-is, but can be used for documentational purposes in test code to emphasize that a certain request should succeed (e.g. allowed by rules).

Initializes a test environment for rules unit testing. Call this function first for test setup.

Requires emulators to be running. This function tries to discover those emulators via environment variables or through the Firebase Emulator hub if hosts and ports are unspecified. It is strongly recommended to specify security rules for emulators used for testing. See minimal example below.

Promise<RulesTestEnvironment>

a promise that resolves with an environment ready for testing, or rejects on error.

Run a setup function with background Cloud Functions triggers disabled. This can be used to import data into the Realtime Database or Cloud Firestore emulator without triggering locally emulated Cloud Functions.

This method only works with Firebase CLI version 8.13.0 or higher. This overload works only if the Emulator hub host:port is specified by the environment variable FIREBASE_EMULATOR_HUB.

Run a setup function with background Cloud Functions triggers disabled. This can be used to import data into the Realtime Database or Cloud Firestore emulator without triggering locally emulated Cloud Functions.

This method only works with Firebase CLI version 8.13.0 or higher. The Emulator hub must be running, which host and port are specified in this overload.

Configuration for a given emulator.

More options for the mock user token to be used for testing, including developer-specfied custom claims or optional overrides for Firebase Auth token payloads.

**Examples:**

Example 1 (javascript):
```javascript
export declare function assertFails(pr: Promise<any>): Promise<any>;
```

Example 2 (javascript):
```javascript
const unauthed = testEnv.unauthenticatedContext();
await assertFails(getDoc(unauthed.firestore(), '/private/doc'), { ... });
```

Example 3 (typescript):
```typescript
export declare function assertSucceeds<T>(pr: Promise<T>): Promise<T>;
```

Example 4 (javascript):
```javascript
const alice = testEnv.authenticatedContext('alice');
await assertSucceeds(getDoc(alice.firestore(), '/doc/readable/by/alice'), { ... });
```

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/GenerationConfig

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- GenerationConfig
  - init(temperature:topP:topK:candidateCount:maxOutputTokens:presencePenalty:frequencyPenalty:stopSequences:responseMIMEType:responseSchema:responseModalities:thinkingConfig:)
    - Declaration
    - Parameters

A struct defining model parameters to be used when sending generative AI requests to the backend model.

Creates a new GenerationConfig value.

See the Configure model parameters guide and the Cloud documentation for more details.

Controls the randomness of the language model’s output. Higher values (for example, 1.0) make the text more random and creative, while lower values (for example, 0.1) make it more focused and deterministic.

Controls diversity of generated text. Higher values (e.g., 0.9) produce more diverse text, while lower values (e.g., 0.5) make the output more focused.

Limits the number of highest probability words the model considers when generating text. For example, a topK of 40 means only the 40 most likely words are considered for the next token. A higher value increases diversity, while a lower value makes the output more deterministic.

The number of response variations to return; defaults to 1 if not set. Support for multiple candidates depends on the model; see the Cloud documentation for more details.

Maximum number of tokens that can be generated in the response. See the configure model parameters documentation for more details.

Controls the likelihood of repeating the same words or phrases already generated in the text. Higher values increase the penalty of repetition, resulting in more diverse output.

Controls the likelihood of repeating words or phrases, with the penalty increasing for each repetition. Higher values increase the penalty of repetition, resulting in more diverse output.

A set of up to 5 Strings that will stop output generation. If specified, the API will stop at the first appearance of a stop sequence. The stop sequence will not be included as part of the response. See the Cloud documentation for more details.

Output response MIME type of the generated candidate text.

Output schema of the generated candidate text. If set, a compatible responseMIMEType must also be set.

The data types (modalities) that may be returned in model responses.

Configuration for controlling the “thinking” behavior of compatible Gemini models; see ThinkingConfig for more details.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Enums/GenerateContentError

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- GenerateContentError
  - internalError(underlying:)
    - Declaration
  - promptImageContentError(underlying:)
    - Declaration
  - promptBlocked(response:)
    - Declaration
  - responseStoppedEarly(reason:response:)
    - Declaration

Errors that occur when generating content from a model.

An internal error occurred. See the underlying error for more context.

An error occurred when constructing the prompt. Examine the related error for details.

A prompt was blocked. See the response’s promptFeedback.blockReason for more information.

A response didn’t fully complete. See the FinishReason for more information.

---

## firebase-admin.project-management package Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/admin/node/firebase-admin.project-management

**Contents:**
- firebase-admin.project-management package Stay organized with collections Save and categorize content based on your preferences.
- Functions
- Classes
- Enumerations
- Interfaces
- Type Aliases
- getProjectManagement(app)
  - Parameters
  - Example 1
  - Example 2

Firebase project management.

Gets the ProjectManagement service for the default app or a given app.

getProjectManagement() can be called with no arguments to access the default app's ProjectManagement service, or as getProjectManagement(app) to access the ProjectManagement service associated with a specific app.

The default ProjectManagement service if no app is provided or the ProjectManagement service associated with the provided app.

Platforms with which a Firebase App can be associated.

**Examples:**

Example 1 (javascript):
```javascript
export declare function getProjectManagement(app?: App): ProjectManagement;
```

Example 2 (javascript):
```javascript
// Get the ProjectManagement service for the default app
const defaultProjectManagement = getProjectManagement();
```

Example 3 (javascript):
```javascript
// Get the ProjectManagement service for a given app
const otherProjectManagement = getProjectManagement(otherApp);
```

Example 4 (typescript):
```typescript
export type ProjectManagementErrorCode = 'already-exists' | 'authentication-error' | 'internal-error' | 'invalid-argument' | 'invalid-project-id' | 'invalid-server-response' | 'not-found' | 'service-unavailable' | 'unknown-error';
```

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/GroundingMetadata/GroundingSupport

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- GroundingSupport
  - segment
    - Declaration
  - groundingChunkIndices
    - Declaration

Provides information about how a specific segment of the model’s response is supported by the retrieved grounding chunks.

Specifies the segment of the model’s response content that this grounding support pertains to.

A list of indices that refer to specific GroundingChunk structs within the groundingChunks array. These referenced chunks are the sources that support the claim made in the associated segment of the response. For example, an array [1, 3, 4] means that groundingChunks[1], groundingChunks[3], groundingChunks[4] are the retrieved content supporting this part of the response.

---

## FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebasecore/api/reference/Type-Definitions

**Contents:**
- FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Type Definitions
  - FIRAppVoidBoolCallback

The following type definitions are available globally.

A block that takes a BOOL and has no return value.

---

## Package index Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/android/packages

**Contents:**
- Package index Stay organized with collections Save and categorize content based on your preferences.
  - ads
  - measurement
  - measurement.api
  - measurement.impl
  - firebase
  - firebase.ai
  - firebase.appcheck
  - firebase.appcheck-debug
  - firebase.appcheck-debug-testing

In July 2025, we stopped releasing new versions of KTX modules for Firebase libraries, and we removed the KTX libraries from the Firebase Android BoM (v34.0.0).

If you use KTX APIs from the previously released KTX modules, we strongly recommend that you migrate your app to use KTX APIs from the main modules instead. For details, see the FAQ about this initiative.

"Vertex AI in Firebase" has been replaced by Firebase AI Logic: firebase-ai

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/AudioTranscriptionConfig

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- AudioTranscriptionConfig
  - init()
    - Declaration

Configuration options for audio transcriptions when communicating with a model that supports the Gemini Live API.

While there are not currently any options, this will likely change in the future. For now, just providing an instance of this struct will enable audio transcriptions for the corresponding input or output fields.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/CountTokensResponse

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- CountTokensResponse
  - totalTokens
    - Declaration
  - promptTokensDetails
    - Declaration
- Codable Conformances
  - init(from:)
    - Declaration

The model’s response to a count tokens request.

The total number of tokens in the input given to the model as a prompt.

The breakdown, by modality, of how many tokens are consumed by the prompt.

---

## FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebasecore/api/reference/Classes/FirebaseConfiguration

**Contents:**
- FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- FirebaseConfiguration
  - shared
    - Declaration
  - setLoggerLevel(_:)
    - Declaration
    - Parameters
  - loggerLevel()
    - Declaration

This interface provides global level properties that the developer can tweak.

Returns the shared configuration object.

Sets the logging level for internal Firebase logging. Firebase will only log messages that are logged at or below loggerLevel. The messages are logged both to the Xcode console and to the device’s log. Note that if an app is running from AppStore, it will never log above .notice even if loggerLevel is set to a higher (more verbose) setting.

The maximum logging level. The default level is set to FIRLoggerLevelNotice.

Returns the logging level for internal Firebase logging.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/GroundingMetadata/GroundingChunk

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- GroundingChunk
  - web
    - Declaration

Represents a chunk of retrieved data that supports a claim in the model’s response. This is part of the grounding information provided when grounding is enabled.

Contains details if the grounding chunk is from a web source.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Classes
  - Chat
    - Declaration
  - FirebaseAI
    - Declaration
  - GenerativeModel
    - Declaration
  - TemplateGenerativeModel
    - Declaration

The following classes are available globally.

An object that represents a back-and-forth chat with a model, capturing the history and saving the context in memory between each message sent.

The Firebase AI SDK provides access to Gemini models directly from your app.

A type that represents a remote multimodal model (like Gemini), with the ability to generate content based on various input types.

A type that represents a remote multimodal model (like Gemini), with the ability to generate content based on various input types.

Public Preview: This API is a public preview and may be subject to change.

A type that represents a remote image generation model (like Imagen), with the ability to generate images based on various input types.

Public Preview: This API is a public preview and may be subject to change.

Represents a remote Imagen model with the ability to generate images using text prompts.

See the generate images documentation for more details about the image generation capabilities offered by the Imagen model in the Firebase AI SDK SDK.

A multimodal model (like Gemini) capable of real-time content generation based on various input types, supporting bidirectional streaming.

You can create a new session via connect().

A live WebSocket session, capable of streaming content to and from the model.

Messages are streamed through responses, and can be sent through either the dedicated realtime API function (such as sendAudioRealtime(_:) and sendTextRealtime(_:)), or through the incremental API (such as sendContent(_:turnComplete:)).

To create an instance of this class, see LiveGenerativeModel.

A Schema object allows the definition of input and output data types.

These types can be objects, but also primitives and arrays. Represents a select subset of an OpenAPI 3.0 schema object.

---

## FirebaseABTesting Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseabtesting/api/reference/Classes/ExperimentController

**Contents:**
- FirebaseABTesting Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- ExperimentController
  - sharedInstance()
    - Declaration
  - -updateExperimentsWithServiceOrigin:events:policy:lastStartTime:payloads:completionHandler:
    - Parameters
  - latestExperimentStartTimestampBetweenTimestamp(_:andPayloads:)
    - Declaration
    - Parameters
  - validateRunningExperiments(forServiceOrigin:running:)

This class is for Firebase services to handle experiments updates to Firebase Analytics. Experiments can be set, cleared and updated through this controller.

Returns the FIRExperimentController singleton.

Updates the list of experiments with an optional completion handler. Experiments already existing in payloads are not affected, whose state and payload is preserved. This method compares whether the experiments have changed or not by their variant ID. This runs in a background queue and calls the completion handler when finished executing.

The originating service affected by the experiment.

A list of event names to be used for logging experiment lifecycle events, if they are not defined in the payload.

The policy to handle new experiments when slots are full.

The last known experiment start timestamp for this affected service. (Timestamps are specified by the number of seconds from 00:00:00 UTC on 1 January 1970.).

List of experiment metadata.

Code to be executed after experiments are updated in the background thread.

Returns the latest experiment start timestamp given a current latest timestamp and a list of experiment payloads. Timestamps are specified by the number of seconds from 00:00:00 UTC on 1 January 1970.

Current latest experiment start timestamp. If not known, affected service should specify -1;

List of experiment metadata.

Expires experiments that aren’t in the list of running experiment payloads.

The originating service affected by the experiment.

The list of valid, running experiments.

Directly sets a given experiment to be active.

The payload for the experiment that should be activated.

The originating service affected by the experiment.

---

## Understand Firebase on Apple platforms Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/ios/learn-more

**Contents:**
- Understand Firebase on Apple platforms Stay organized with collections Save and categorize content based on your preferences.
- Firebase library support by platform
  - App Clips
- GoogleService-Info.plist
- Swift Package Manager
- Swift Extensions
- SwiftUI
- App delegate swizzling
- Supporting iOS 14
- Ongoing support for Objective-C

As you're developing your Apple app using Firebase, you might discover concepts that are unfamiliar or specific to Firebase. This page aims to answer those questions or point you to resources to learn more.

If you have questions about a topic not covered on this page, feel free to visit one of our online communities. We'll also update this page with new topics periodically, so check back to see if we've added the topic you want to learn about!

The following table describes which Firebase libraries are compatible with which Apple platforms. For the time being, visionOS and watchOS are community-supported only. See the Firebase Apple platforms SDK GitHub repository for installation instructions and known issues.

1 Firebase AI Logic was formerly called "Vertex AI in Firebase".

Most Firebase libraries will build and run in an App Clip target, however, many are restricted as a result of underlying OS restrictions. Known issues include:

See the Firebase GitHub repository for a full list of known App Clip issues.

As part of adding Firebase to your Apple project, you need to add the GoogleService-Info.plist configuration file to your project. If you want to use multiple Firebase projects in a single app, visit the documentation for configuring multiple projects.

See the Swift reference documentation to learn about the Firebase app initialization process in more detail.

Learn more about Swift Package Manager integration in our guide.

Firebase Apple platform SDK Swift extensions were formerly small, open source add-ons to the existing Firebase Apple platform libraries that enable your code to use Swift language-specific features. These APIs have since been added directly to the main libraries and don't need to be included separately. If you formerly had a Swift extension SDK in your codebase, see the migration guide for upgrade instructions.

Firebase fully supports SwiftUI, though the setup will be slightly different from UIKit apps in order for Firebase to function correctly in a fully SwiftUI environment. Take a look at this blog post by Peter Friese for more details.

SwiftUI applications must disable swizzling due to a known issue. See the app delegate swizzling section for more details.

Firebase swizzles some methods in your app's app delegate class to automatically connect certain Firebase services to OS callbacks, like FCM and the APNs token. You can disable swizzling in your app by adding the flag FirebaseAppDelegateProxyEnabled in the app’s Info.plist file and setting it to NO.

Four Firebase products use App Delegate swizzling: Analytics, App Distribution, Authentication, and FCM. If you've disabled swizzling in your application and you use any of the following products, refer to the product-specific guide to learn about how to use the product without swizzling:

iOS 14 includes new changes to user permissions surrounding the user's advertising identifier. See the preparing for iOS 14 guide for more details on whether or not your app may be affected.

To ease maintenance of our Apple platforms documentation, Firebase has decided to concentrate on Swift snippets and code samples in our guides and other developer materials. Objective-C snippets will be removed from our guides starting January 1, 2024. We will continue to maintain up-to-date reference documentation for Objective-C for all Firebase products.

Firebase supports open source development, and we encourage community contributions and feedback.

All Firebase SDKs for Apple platforms except Analytics are developed as open source libraries in our public Firebase GitHub repository.

FirebaseUI is a set of utility libraries built on Firebase, including a drop-in UI flow for authentication and data utilities for Cloud Firestore and Realtime Database. See more details about FirebaseUI on our GitHub page.

Firebase maintains a collection of quickstart samples for most Firebase APIs on iOS. Find these quickstarts in our public Firebase GitHub quickstart repository.

You can open each quickstart in Xcode, then run them on a mobile device or simulator. Or you can use these quickstarts as example code for using Firebase SDKs.

---

## FirebaseABTesting Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseabtesting/api/reference/Enums

**Contents:**
- FirebaseABTesting Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Enumerations
  - ABTExperimentPayloadExperimentOverflowPolicy

The following enumerations are available globally.

---

## firebase_admin.project_management module Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/admin/python/firebase_admin.project_management

**Contents:**
- firebase_admin.project_management module Stay organized with collections Save and categorize content based on your preferences.
- Classes
  - AndroidApp
  - AndroidAppMetadata
  - IOSApp
  - IOSAppMetadata
  - SHACertificate
- Functions
  - android_app
  - create_android_app

Firebase Project Management module.

This module enables management of resources in Firebase projects, such as Android and iOS apps.

A reference to an Android app within a Firebase project.

Note: Unless otherwise specified, all methods defined in this class make an RPC.

Please use the module-level function android_app(app_id) to obtain instances of this class instead of instantiating it directly.

Adds a SHA certificate to this Android app.

certificate_to_add – The SHA certificate to add.

FirebaseError – If an error occurs while communicating with the Firebase Project Management Service. (For example, if the certificate_to_add already exists.)

Removes a SHA certificate from this Android app.

certificate_to_delete – The SHA certificate to delete.

FirebaseError – If an error occurs while communicating with the Firebase Project Management Service. (For example, if the certificate_to_delete is not found.)

Retrieves the configuration artifact associated with this Android app.

Retrieves detailed information about this Android app.

An AndroidAppMetadata instance.

FirebaseError – If an error occurs while communicating with the Firebase Project Management Service.

Retrieves the entire list of SHA certificates associated with this Android app.

A list of SHACertificate instances.

FirebaseError – If an error occurs while communicating with the Firebase Project Management Service.

Updates the display name attribute of this Android app to the one given.

new_display_name – The new display name for this Android app.

FirebaseError – If an error occurs while communicating with the Firebase Project Management Service.

Returns the app ID of the Android app to which this instance refers.

Note: This method does not make an RPC.

The app ID of the Android app to which this instance refers.

Android-specific information about an Android Firebase app.

The canonical package name of this Android app as it would appear in the Play Store.

A reference to an iOS app within a Firebase project.

Note: Unless otherwise specified, all methods defined in this class make an RPC.

Please use the module-level function ios_app(app_id) to obtain instances of this class instead of instantiating it directly.

Retrieves the configuration artifact associated with this iOS app.

Retrieves detailed information about this iOS app.

An IOSAppMetadata instance.

FirebaseError – If an error occurs while communicating with the Firebase Project Management Service.

Updates the display name attribute of this iOS app to the one given.

new_display_name – The new display name for this iOS app.

FirebaseError – If an error occurs while communicating with the Firebase Project Management Service.

Returns the app ID of the iOS app to which this instance refers.

Note: This method does not make an RPC.

The app ID of the iOS app to which this instance refers.

iOS-specific information about an iOS Firebase app.

The canonical bundle ID of this iOS app as it would appear in the iOS AppStore.

Represents a SHA-1 or SHA-256 certificate associated with an Android app.

Returns the type of the SHA certificate encoded in the hash.

One of ‘SHA_1’ or ‘SHA_256’.

Returns the fully qualified resource name of this certificate, if known.

The fully qualified resource name of this certificate, if known; otherwise, the empty string.

Returns the certificate hash.

The certificate hash.

Obtains a reference to an Android app in the associated Firebase project.

app_id – The app ID that identifies this Android app.

app – An App instance (optional).

An AndroidApp instance.

Creates a new Android app in the associated Firebase project.

package_name – The package name of the Android app to be created.

display_name – A nickname for this Android app (optional).

app – An App instance (optional).

An AndroidApp instance that is a reference to the newly created app.

Creates a new iOS app in the associated Firebase project.

bundle_id – The bundle ID of the iOS app to be created.

display_name – A nickname for this iOS app (optional).

app – An App instance (optional).

An IOSApp instance that is a reference to the newly created app.

Obtains a reference to an iOS app in the associated Firebase project.

app_id – The app ID that identifies this iOS app.

app – An App instance (optional).

Lists all Android apps in the associated Firebase project.

app – An App instance (optional).

a list of AndroidApp instances referring to each Android app in the Firebase project.

Lists all iOS apps in the associated Firebase project.

app – An App instance (optional).

a list of IOSApp instances referring to each iOS app in the Firebase project.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/GroundingMetadata/SearchEntryPoint

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- SearchEntryPoint
  - renderedContent
    - Declaration

A struct representing the Google Search entry point.

An HTML/CSS snippet that can be embedded in your app.

To ensure proper rendering, it’s recommended to display this content within a WKWebView.

---

## TestEnvironmentConfig interface Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/emulator-suite/rules-unit-testing/rules-unit-testing.testenvironmentconfig

**Contents:**
- TestEnvironmentConfig interface Stay organized with collections Save and categorize content based on your preferences.
- Properties
- TestEnvironmentConfig.database
- TestEnvironmentConfig.firestore
- TestEnvironmentConfig.hub
- TestEnvironmentConfig.projectId
- TestEnvironmentConfig.storage

The Database emulator. Its host and port can also be discovered automatically through the hub (see field "hub") or specified via the environment variable FIREBASE_DATABASE_EMULATOR_HOST.

The Firestore emulator. Its host and port can also be discovered automatically through the hub (see field "hub") or specified via the environment variable FIRESTORE_EMULATOR_HOST.

The Firebase Emulator hub. Can also be specified via the environment variable FIREBASE_EMULATOR_HUB. If specified either way, other running emulators can be automatically discovered, and thus do not to be explicity specified.

The project ID of the test environment. Can also be specified via the environment variable GCLOUD_PROJECT.

A "demo-*" project ID is strongly recommended, especially for unit testing. See: https://firebase.google.com/docs/emulator-suite/connect_firestore#choose_a_firebase_project

The Storage emulator. Its host and port can also be discovered automatically through the hub (see field "hub") or specified via the environment variable FIREBASE_STORAGE_EMULATOR_HOST.

**Examples:**

Example 1 (typescript):
```typescript
export interface TestEnvironmentConfig
```

Example 2 (unknown):
```unknown
database?: EmulatorConfig;
```

Example 3 (unknown):
```unknown
firestore?: EmulatorConfig;
```

Example 4 (unknown):
```unknown
hub?: HostAndPort;
```

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/GoogleSearch

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- GoogleSearch
  - init()
    - Declaration

A tool that allows the generative model to connect to Google Search to access and incorporate up-to-date information from the web into its responses.

When using this feature, you are required to comply with the “Grounding with Google Search” usage requirements for your chosen API provider: Gemini Developer API or Vertex AI Gemini API (see Service Terms section within the Service Specific Terms).

---

## FirebaseABTesting Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseabtesting/api/reference/Constants

**Contents:**
- FirebaseABTesting Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Constants
  - FIRDefaultExperimentOverflowPolicy
    - Declaration
  - DefaultSetExperimentEventName
    - Declaration
  - DefaultActivateExperimentEventName
    - Declaration
  - DefaultClearExperimentEventName
    - Declaration

The following constants are available globally.

The default experiment overflow policy, that is to discard the experiment with the oldest start time when users start the experiment on the web console.

Default event name for when an experiment is set.

Default event name for when an experiment is activated.

Default event name for when an experiment is cleared.

Default event name for when an experiment times out for being activated.

Default event name for when an experiment is expired as it reaches the end of TTL.

---

## Migrate to using the Swift extension APIs in the main modules Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/ios/swift-migration

**Contents:**
- Migrate to using the Swift extension APIs in the main modules Stay organized with collections Save and categorize content based on your preferences.
- What's changing?
- Important dates for this change
  - In October 2023
  - As early as February 2024
- How to migrate to use Swift-native APIs from the main module
  - Workspace changes
    - Swift Package Manager
    - CocoaPods
    - Zip distribution and Carthage

We're merging our Swift extension SDKs into the main SDKs in order to make Swift-native APIs more broadly available and increase our ability to support new Swift language features in the future. The changes we're making and their expected impacts on your projects are documented below.

Starting with Firebase for Apple SDK 10.17.0, the Swift extension SDKs have been merged into their corresponding main SDKs. For example, all of the APIs from the FirebaseFirestoreSwift module have been added to FirebaseFirestore, so you no longer have to import the FirebaseFirestoreSwift module to access those APIs.

As all Swift extensions now are part of the main modules, the extension SDKs are no longer required, and are deprecated. Including or using the Swift extension SDKs will raise a compiler warning and as early as February 2024, we'll stop releasing the Swift extensions entirely.

★ Note: Any currently or previously released versions of the Swift extensions will still function. However, we recommend that you migrate your app to use Swift APIs from the main module to ensure you continue to receive fixes and can take advantage of changes and new features.

The Swift extension SDKs have been merged into the main SDKs and then deprecated in favor of the main SDKs. See the release notes for version 10.17.0 announcing this change.

You can now use the Swift extension SDK APIs directly from the main SDK modules. Usage of the extension SDKs will is still possible until the next major version release but will raise a deprecation warning when used.

We'll stop releasing new versions of the Swift extensions, and we'll remove the Swift extensions from Firebase's Package.swift. Older versions will continue to function but will not receive updates.

If you currently do not use the Swift extension SDKs, no action is necessary. If you do use a Swift extension SDK, make the following changes in your project.

After updating Firebase to version 10.17.0+, navigate to the Frameworks, Libraries, and Embedded Content section in the General tab of your target's settings and remove the Swift extension SDK (such as FirebaseFirestoreSwift).

After updating Firebase to version 10.17.0+, navigate to your Podfile and remove the line corresponding to your project's dependency on adding the frameworks section for your target and remove the Swift extension SDK (such as pod FirebaseFirestoreSwift). Then, re-run the pod install command.

After updating Firebase to version 10.17.0+, remove any Swift extension xcframeworks within your project (such as FirebaseFirestoreSwift.xcframework).

For all of the Swift extension SDKs you previously used, take the following actions:

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/FunctionCallPart

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- FunctionCallPart
  - name
    - Declaration
  - args
    - Declaration
  - isThought
    - Declaration
  - functionId
    - Declaration

A predicted function call returned from the model.

The name of the function to call.

The function parameters and values.

Unique id of the function call.

If present, the returned FunctionResponsePart should have a matching functionId field.

Constructs a new function call part.

A FunctionCallPart is typically received from the model, rather than created manually.

The name of the function to call.

The function parameters and values.

Constructs a new function call part.

A FunctionCallPart is typically received from the model, rather than created manually.

The name of the function to call.

The function parameters and values.

Unique id of the function call. If present, the returned FunctionResponsePart should have a matching functionId field.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes/Schema

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Schema
  - StringFormat
    - Declaration
  - IntegerFormat
    - Declaration
  - type
    - Declaration
  - format
    - Declaration

A Schema object allows the definition of input and output data types.

These types can be objects, but also primitives and arrays. Represents a select subset of an OpenAPI 3.0 schema object.

Modifiers describing the expected format of a string Schema.

Modifiers describing the expected format of an integer Schema.

The format of the data.

A human-readable explanation of the purpose of the schema or property. While not strictly enforced on the value itself, good descriptions significantly help the model understand the context and generate more relevant and accurate output.

A human-readable name/summary for the schema or a specific property. This helps document the schema’s purpose but doesn’t typically constrain the generated value. It can subtly guide the model by clarifying the intent of a field.

Indicates if the value may be null.

Possible values of the element of type “STRING” with “enum” format.

Defines the schema for the elements within the "ARRAY". All items in the generated array must conform to this schema definition. This can be a simple type (like .string) or a complex nested object schema.

An integer specifying the minimum number of items the generated "ARRAY" must contain.

An integer specifying the maximum number of items the generated "ARRAY" must contain.

The minimum value of a numeric type.

The maximum value of a numeric type.

Defines the members (key-value pairs) expected within an object. It’s a dictionary where keys are the property names (strings) and values are nested Schema definitions describing each property’s type and constraints.

An array of Schema objects. The generated data must be valid against any (one or more) of the schemas listed in this array. This allows specifying multiple possible structures or types for a single field.

For example, a value could be either a String or an Integer:

An array of strings, where each string is the name of a property defined in the properties dictionary that must be present in the generated object. If a property is listed here, the model must include it in the output.

A specific hint provided to the Gemini model, suggesting the order in which the keys should appear in the generated JSON string. Important: Standard JSON objects are inherently unordered collections of key-value pairs. While the model will try to respect propertyOrdering in its textual JSON output, subsequent parsing into native Swift objects (like Dictionaries or Structs) might not preserve this order. This parameter primarily affects the raw JSON string serialization.

Returns a Schema representing a string value.

This schema instructs the model to produce data of type "STRING", which is suitable for decoding into a Swift String (or String?, if nullable is set to true).

If a specific set of string values should be generated by the model (for example, “north”, “south”, “east”, or “west”), use enumeration(values:description:nullable:) instead to constrain the generated values.

An optional description of what the string should contain or represent; may use Markdown format.

An optional human-readable name/summary for the schema.

If true, instructs the model that it may generate null instead of a string; defaults to false, enforcing that a string value is generated.

An optional modifier describing the expected format of the string. Currently no formats are officially supported for strings but custom values may be specified using custom(_:), for example .custom("email") or .custom("byte"); these provide additional hints for how the model should respond but are not guaranteed to be adhered to.

Returns a Schema representing an enumeration of string values.

This schema instructs the model to produce data of type "STRING" with the format "enum". This data is suitable for decoding into a Swift String (or String?, if nullable is set to true), or an enum with strings as raw values.

Example: The values ["north", "south", "east", "west"] for an enumeration of directions.

The list of string values that may be generated by the model.

An optional description of what the values contain or represent; may use Markdown format.

An optional human-readable name/summary for the schema.

If true, instructs the model that it may generate null instead of one of the strings specified in values; defaults to false, enforcing that one of the string values is generated.

Returns a Schema representing a single-precision floating-point number.

This schema instructs the model to produce data of type "NUMBER" with the format "float", which is suitable for decoding into a Swift Float (or Float?, if nullable is set to true).

This Schema provides a hint to the model that it should generate a single-precision floating-point number, a float, but only guarantees that the value will be a number.

An optional description of what the number should contain or represent; may use Markdown format.

An optional human-readable name/summary for the schema.

If true, instructs the model that it may generate null instead of a number; defaults to false, enforcing that a number is generated.

If specified, instructs the model that the value should be greater than or equal to the specified minimum.

If specified, instructs the model that the value should be less than or equal to the specified maximum.

Returns a Schema representing a floating-point number.

This schema instructs the model to produce data of type "NUMBER", which is suitable for decoding into a Swift Double (or Double?, if nullable is set to true).

An optional description of what the number should contain or represent; may use Markdown format.

An optional human-readable name/summary for the schema.

If true, instructs the model that it may return null instead of a number; defaults to false, enforcing that a number is returned.

If specified, instructs the model that the value should be greater than or equal to the specified minimum.

If specified, instructs the model that the value should be less than or equal to the specified maximum.

Returns a Schema representing an integer value.

This schema instructs the model to produce data of type "INTEGER", which is suitable for decoding into a Swift Int (or Int?, if nullable is set to true) or other integer types (such as Int32) based on the expected size of values being generated.

If a format of int32 or int64 is specified, this provides a hint to the model that it should generate 32-bit or 64-bit integers but this Schema only guarantees that the value will be an integer. Therefore, it is possible that decoding into an Int32 could overflow even if a format of int32 is specified.

An optional description of what the integer should contain or represent; may use Markdown format.

An optional human-readable name/summary for the schema.

If true, instructs the model that it may return null instead of an integer; defaults to false, enforcing that an integer is returned.

An optional modifier describing the expected format of the integer. Currently the formats int32 and int64 are supported; custom values may be specified using custom(_:) but may be ignored by the model.

If specified, instructs the model that the value should be greater than or equal to the specified minimum.

If specified, instructs the model that the value should be less than or equal to the specified maximum.

Returns a Schema representing a boolean value.

This schema instructs the model to produce data of type "BOOLEAN", which is suitable for decoding into a Swift Bool (or Bool?, if nullable is set to true).

An optional description of what the boolean should contain or represent; may use Markdown format.

An optional human-readable name/summary for the schema.

If true, instructs the model that it may return null instead of a boolean; defaults to false, enforcing that a boolean is returned.

Returns a Schema representing an array.

This schema instructs the model to produce data of type "ARRAY", which has elements of any other data type (including nested "ARRAY"s). This data is suitable for decoding into many Swift collection types, including Array, holding elements of types suitable for decoding from the respective items type.

The Schema of the elements that the array will hold.

An optional description of what the array should contain or represent; may use Markdown format.

An optional human-readable name/summary for the schema.

If true, instructs the model that it may return null instead of an array; defaults to false, enforcing that an array is returned.

Instructs the model to produce at least the specified minimum number of elements in the array; defaults to nil, meaning any number.

Instructs the model to produce at most the specified maximum number of elements in the array.

Returns a Schema representing an object.

This schema instructs the model to produce data of type "OBJECT", which has keys of type "STRING" and values of any other data type (including nested "OBJECT"s). This data is suitable for decoding into Swift keyed collection types, including Dictionary, or other custom struct or class types.

Example: A City could be represented with the following object Schema.

The generated data could be decoded into a Swift native type:

A dictionary containing the object’s property names as keys and their respective Schemas as values.

A list of property names that may be be omitted in objects generated by the model; these names must correspond to the keys provided in the properties dictionary and may be an empty list.

An optional hint to the model suggesting the order for keys in the generated JSON string. See propertyOrdering for details.

An optional description of what the object should contain or represent; may use Markdown format.

An optional human-readable name/summary for the schema.

If true, instructs the model that it may return null instead of an object; defaults to false, enforcing that an object is returned.

Returns a Schema representing a value that must conform to any (one or more) of the provided sub-schemas.

This schema instructs the model to produce data that is valid against at least one of the schemas listed in the schemas array. This is useful when a field can accept multiple distinct types or structures.

Example: A field that can hold either a simple user ID (integer) or a detailed user object.

The generated data could be decoded based on which schema it matches.

An array of Schema objects. The generated data must be valid against at least one of these schemas. The array must not be empty.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes/TemplateGenerativeModel

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- TemplateGenerativeModel
  - generateContent(templateID:inputs:options:)
    - Declaration
    - Parameters
    - Return Value
  - generateContentStream(templateID:inputs:options:)
    - Declaration
    - Parameters
    - Return Value

A type that represents a remote multimodal model (like Gemini), with the ability to generate content based on various input types.

Public Preview: This API is a public preview and may be subject to change.

Generates content from a prompt template and inputs.

Public Preview: This API is a public preview and may be subject to change.

The ID of the prompt template to use.

A dictionary of variables to substitute into the template.

The RequestOptions for the request, currently used to override default request timeout.

The content generated by the model.

Generates content from a prompt template and inputs, with streaming responses.

Public Preview: This API is a public preview and may be subject to change.

The ID of the prompt template to use.

A dictionary of variables to substitute into the template.

The RequestOptions for the request, currently used to override default request timeout.

An AsyncThrowingStream that yields GenerateContentResponse objects.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/ImagenGenerationResponse

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- ImagenGenerationResponse
  - images
    - Declaration
  - filteredReason
    - Declaration
- Available where `T`: `Decodable`
  - init(from:)
    - Declaration

A response from a request to generate images with Imagen.

The type placeholder T is an image type; this is currently always an ImagenInlineImage.

This type is returned from:

The images generated by Imagen; see ImagenInlineImage.

The number of images generated may be fewer than the number requested if one or more were filtered out; see filteredReason.

The reason, if any, that generated images were filtered out.

This property will only be populated if fewer images were generated than were requested, otherwise it will be nil. Images may be filtered out due to the ImagenSafetyFilterLevel, the ImagenPersonFilterLevel, or filtering included in the model. The filter levels may be adjusted in your ImagenSafetySettings. See the Responsible AI and usage guidelines for Imagen for more details.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Extensions/CGImage

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- CGImage
  - partsValue
    - Declaration

Enables CGImages to be representable as model content.

---

## Options to install Firebase in your Apple app Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/ios/installation-methods

**Contents:**
- Options to install Firebase in your Apple app Stay organized with collections Save and categorize content based on your preferences.
- Swift Package Manager
  - Via Xcode
  - Via Package.swift
  - Product-specific considerations
    - Google Analytics
    - Crashlytics
- CocoaPods
  - Analytics enabled
  - Analytics not enabled

Firebase recommends Swift Package Manager for new projects.

Swift Package Manager support requires 16.2 or higher.

If migrating from a CocoaPods-based project, run pod deintegrate to remove CocoaPods from your Xcode project. The CocoaPods-generated .xcworkspace file can safely be deleted afterward. If you're adding Firebase to a project for the first time, this step can be ignored.

In Xcode, install the Firebase libraries by navigating to File > Add Packages.

In the prompt that appears, select the Firebase GitHub repository:

Select the version of Firebase you want to use. For new projects, we recommend using the newest version of Firebase.

Choose the Firebase libraries you want to include in your app.

Once you're finished, Xcode will begin resolving your package dependencies and downloading them in the background.

To integrate Firebase to a Swift package via a Package.swift manifest, you can add Firebase to the dependencies array of your package. For more details, see the Swift Package Manager documentation.

Then in any target that depends on a Firebase product, add it to the dependencies array of that target.

Some Firebase products require extra integration steps in order to function correctly.

Google Analytics requires adding the -ObjC linker flag to your target's build settings if included transitively.

Crashlytics requires you to upload debug symbols.

You can use a run script build phase for Xcode to automatically upload debug symbols post-build. Find the run script here:

Another option for uploading symbols is to use the upload-symbols script. Place the script in a subdirectory of your project file (for example scripts/upload-symbols), then make sure that the script is executable:

This script can be used to manually upload dSYM files. For usage notes and additional instructions for the script, run upload-symbols without any parameters.

Firebase supports installation with CocoaPods in addition to Swift Package Manager.

Firebase's CocoaPods distribution requires Xcode 16.2 and CocoaPods 1.12.0 or higher. Here's how to install Firebase using CocoaPods:

Create a Podfile if you don't already have one. From the root of your project directory, run the following command:

To your Podfile, add the Firebase pods that you want to use in your app.

You can add any of the supported Firebase products to your app.

Learn more about IDFA, the device-level advertising identifier, in Apple's User Privacy and Data Use and App Tracking Transparency documentation.

Install the pods, then open your .xcworkspace file to see the project in Xcode:

Some Firebase products require extra integration steps in order to function correctly.

Crashlytics requires you to upload debug symbols.

You can use a run script build phase for Xcode to automatically upload debug symbols post-build. Find the run script here:

Carthage support is experimental. See the instructions on GitHub for including Firebase in your app via Carthage.

Firebase provides a pre-built binary XCFramework distribution for users who want to integrate Firebase without using a dependency manager. To install Firebase:

Download the framework SDK zip. This file contains architecture slices for all available target architectures for all Firebase SDKs and thus may take some time to download.

Unzip the file, then review the README for the frameworks that you want to include in your app.

Add the -ObjC linker flag in your Other Linker Settings in your target's build settings.

**Examples:**

Example 1 (yaml):
```yaml
https://github.com/firebase/firebase-ios-sdk.git
```

Example 2 (yaml):
```yaml
dependencies: [

  .package(name: "Firebase",
           url: "https://github.com/firebase/firebase-ios-sdk.git",
           from: "8.0"),
  // ...

],
```

Example 3 (json):
```json
.target(
  name: "MyTargetName",
  dependencies: [
    .product(name: "FirebaseAuth", package: "Firebase"),
    // ...
  ]
),
```

Example 4 (bash):
```bash
${BUILD_DIR%Build/*}/SourcePackages/checkouts/firebase-ios-sdk/Crashlytics/run
```

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes/GenerativeModel

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- GenerativeModel
  - generateContent(_:)
    - Declaration
    - Parameters
    - Return Value
  - generateContent(_:)
    - Declaration
    - Parameters
    - Return Value

A type that represents a remote multimodal model (like Gemini), with the ability to generate content based on various input types.

Generates content from String and/or image inputs, given to the model as a prompt, that are representable as one or more Parts.

Since Parts do not specify a role, this method is intended for generating content from zero-shot or “direct” prompts. For few-shot prompts, see generateContent(_ content: [ModelContent]).

The input(s) given to the model as a prompt (see PartsRepresentable for conforming types).

The content generated by the model.

Generates new content from input content given to the model as a prompt.

The input(s) given to the model as a prompt.

The generated content response from the model.

Generates content from String and/or image inputs, given to the model as a prompt, that are representable as one or more Parts.

Since Parts do not specify a role, this method is intended for generating content from zero-shot or “direct” prompts. For few-shot prompts, see generateContentStream(_ content: @autoclosure () throws -> [ModelContent]).

The input(s) given to the model as a prompt (see PartsRepresentable for conforming types).

A stream wrapping content generated by the model or a GenerateContentError error if an error occurred.

Generates new content from input content given to the model as a prompt.

The input(s) given to the model as a prompt.

A stream wrapping content generated by the model or a GenerateContentError error if an error occurred.

Creates a new chat conversation using this model with the provided history.

Runs the model’s tokenizer on String and/or image inputs that are representable as one or more Parts.

Since Parts do not specify a role, this method is intended for tokenizing zero-shot or “direct” prompts. For few-shot input, see countTokens(_ content: @autoclosure () throws -> [ModelContent]).

The input(s) given to the model as a prompt (see PartsRepresentable for conforming types).

The results of running the model’s tokenizer on the input; contains totalTokens.

Runs the model’s tokenizer on the input content and returns the token count.

The input given to the model as a prompt.

The results of running the model’s tokenizer on the input; contains totalTokens.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/FileDataPart

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- FileDataPart
  - uri
    - Declaration
  - mimeType
    - Declaration
  - isThought
    - Declaration
  - init(uri:mimeType:)
    - Declaration

File data stored in Cloud Storage for Firebase, referenced by URI.

Constructs a new file data part.

The "gs://"-prefixed URI of the file in Cloud Storage for Firebase, for example, "gs://bucket-name/path/image.jpg".

The IANA standard MIME type of the uploaded file, for example, "image/jpeg" or "video/mp4"; see supported input files and requirements for supported values.

---

## FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebasecore/api/reference/Classes/FirebaseApp

**Contents:**
- FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- FirebaseApp
  - configure()
    - Declaration
  - configure(options:)
    - Declaration
    - Parameters
  - configure(name:options:)
    - Declaration
    - Parameters

The entry point of Firebase SDKs.

Initialize and configure FirebaseApp using FirebaseApp.configure() or other customized ways as shown below.

The logging system has two modes: default mode and debug mode. In default mode, only logs with log level Notice, Warning and Error will be sent to device. In debug mode, all logs will be sent to device. The log levels that Firebase uses are consistent with the ASL log levels.

Enable debug mode by passing the -FIRDebugEnabled argument to the application. You can add this argument in the application’s Xcode scheme. When debug mode is enabled via -FIRDebugEnabled, further executions of the application will also be in debug mode. In order to return to default mode, you must explicitly disable the debug mode with the application argument -FIRDebugDisabled.

It is also possible to change the default logging level in code by calling FirebaseConfiguration.shared.setLoggerLevel(_:) with the desired level.

Configures a default Firebase app. Raises an exception if any configuration step fails. The default app is named “__FIRAPP_DEFAULT”. This method should be called after the app is launched and before using Firebase services. This method should be called from the main thread and contains synchronous file I/O (reading GoogleService-Info.plist from disk).

Configures the default Firebase app with the provided options. The default app is named “__FIRAPP_DEFAULT”. Raises an exception if any configuration step fails. This method should be called from the main thread.

The Firebase application options used to configure the service.

Configures a Firebase app with the given name and options. Raises an exception if any configuration step fails. This method should be called from the main thread.

The application’s name given by the developer. The name should should only contain Letters, Numbers and Underscore.

The Firebase application options used to configure the services.

Returns the default app, or nil if the default app does not exist.

Returns a previously created FirebaseApp instance with the given name, or nil if no such app exists. This method is thread safe.

Returns the set of all extant FirebaseApp instances, or nil if there are no FirebaseApp instances. This method is thread safe.

Cleans up the current FirebaseApp, freeing associated data and returning its name to the pool for future use. This method is thread safe.

FirebaseApp instances should not be initialized directly. Call FirebaseApp.configure(), FirebaseApp.configure(options:), or FirebaseApp.configure(name:options:) directly.

Gets the name of this app.

Gets a copy of the options for this app. These are non-modifiable.

Gets or sets whether automatic data collection is enabled for all products. Defaults to true unless FirebaseDataCollectionDefaultEnabled is set to NO in your app’s Info.plist. This value is persisted across runs of the app so that it can be set once when users have consented to collection.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/FinishReason

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- FinishReason
  - stop
    - Declaration
  - maxTokens
    - Declaration
  - safety
    - Declaration
  - recitation
    - Declaration

A value enumerating possible reasons for a model to terminate a content generation request.

Natural stop point of the model or provided stop sequence.

The maximum number of tokens as specified in the request was reached.

The token generation was stopped because the response was flagged for safety reasons.

When streaming, the content will be empty if content filters blocked the output.

The token generation was stopped because the response was flagged for unauthorized citations.

All other reasons that stopped token generation.

Token generation was stopped because the response contained forbidden terms.

Token generation was stopped because the response contained potentially prohibited content.

Token generation was stopped because of Sensitive Personally Identifiable Information (SPII).

Token generation was stopped because the function call generated by the model was invalid.

Returns the raw string representation of the FinishReason value.

This value directly corresponds to the values in the REST API.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes/TemplateImagenModel

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- TemplateImagenModel
  - generateImages(templateID:inputs:options:)
    - Declaration
    - Parameters
    - Return Value

A type that represents a remote image generation model (like Imagen), with the ability to generate images based on various input types.

Public Preview: This API is a public preview and may be subject to change.

Generates images from a prompt template and variables.

The prompt template to use.

A dictionary of variables to substitute into the template.

The RequestOptions for the request, currently used to override default request timeout.

The images generated by the model.

---

## com.google.firebase.projectmanagement Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/admin/java/reference/com/google/firebase/projectmanagement/package-summary

**Contents:**
- com.google.firebase.projectmanagement Stay organized with collections Save and categorize content based on your preferences.
  - Classes
  - Enums
  - Exceptions

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/CodeExecutionResultPart/Outcome

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Outcome
  - ok
    - Declaration
  - failed
    - Declaration
  - deadlineExceeded
    - Declaration
  - description
    - Declaration

The outcome of a code execution.

The code executed without errors.

The code failed to execute.

The code took too long to execute.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Protocols/Part

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Part
  - isThought
    - Declaration
  - partsValue
    - Declaration

A discrete piece of data in a media format interpretable by an AI model.

Within a single value of Part, different data types may not mix.

Indicates whether this Part is a summary of the model’s internal thinking process.

When includeThoughts is set to true in ThinkingConfig, the model may return one or more “thought” parts that provide insight into how it reasoned through the prompt to arrive at the final answer. These parts will have isThought set to true.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/FunctionCallingConfig

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- FunctionCallingConfig
  - auto()
    - Declaration
  - any(allowedFunctionNames:)
    - Declaration
    - Parameters
  - none()
    - Declaration

Configuration for specifying function calling behavior.

Creates a function calling config where the model calls functions at its discretion.

This is the default behavior.

Creates a function calling config where the model will always call a provided function.

A set of function names that, when provided, limits the functions that the model will call.

Creates a function calling config where the model will never call a function.

This can also be achieved by not passing any FunctionDeclaration tools when instantiating the model.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes/Chat

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Chat
  - history
    - Declaration
  - sendMessage(_:)
    - Declaration
    - Parameters
    - Return Value
  - sendMessage(_:)
    - Declaration

An object that represents a back-and-forth chat with a model, capturing the history and saving the context in memory between each message sent.

The previous content from the chat that has been successfully sent and received from the model. This will be provided to the model for each message sent as context for the discussion.

Sends a message using the existing history of this chat as context. If successful, the message and response will be added to the history. If unsuccessful, history will remain unchanged.

The new content to send as a single chat message.

The model’s response if no error occurred.

Sends a message using the existing history of this chat as context. If successful, the message and response will be added to the history. If unsuccessful, history will remain unchanged.

The new content to send as a single chat message.

The model’s response if no error occurred.

Sends a message using the existing history of this chat as context. If successful, the message and response will be added to the history. If unsuccessful, history will remain unchanged.

The new content to send as a single chat message.

A stream containing the model’s response or an error if an error occurred.

Sends a message using the existing history of this chat as context. If successful, the message and response will be added to the history. If unsuccessful, history will remain unchanged.

The new content to send as a single chat message.

A stream containing the model’s response or an error if an error occurred.

---

## FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebasecore/api/reference/Enums/FIRLoggerLevel

**Contents:**
- FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- FIRLoggerLevel
  - FIRLoggerLevelError
  - FIRLoggerLevelWarning
  - FIRLoggerLevelNotice
  - FIRLoggerLevelInfo
  - FIRLoggerLevelDebug
  - FIRLoggerLevelMin
  - FIRLoggerLevelMax

The log levels used by internal logging.

Error level, matches ASL_LEVEL_ERR.

Warning level, matches ASL_LEVEL_WARNING.

Notice level, matches ASL_LEVEL_NOTICE.

Info level, matches ASL_LEVEL_INFO.

Debug level, matches ASL_LEVEL_DEBUG.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes/ImagenModel

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- ImagenModel
  - generateImages(prompt:)
    - Declaration
    - Parameters

Represents a remote Imagen model with the ability to generate images using text prompts.

See the generate images documentation for more details about the image generation capabilities offered by the Imagen model in the Firebase AI SDK SDK.

Generates images using the Imagen model and returns them as inline data.

The individual data is provided for each of the generated images.

By default, 1 image sample is generated; see numberOfImages to configure the number of images that are generated.

A text prompt describing the image(s) to generate.

---

## Module Index Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/modules

**Contents:**
- Module Index Stay organized with collections Save and categorize content based on your preferences.

---

## Firebase API Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/

**Contents:**
- Firebase API Reference Stay organized with collections Save and categorize content based on your preferences.
  - Apple platforms
  - Android
  - Web
  - Flutter
  - Unity
  - C++
  - Admin
- Other reference documentation
- Index of all libraries

The API reference documentation provides detailed information for each of the classes and methods in the Firebase SDK. Choose your preferred platform from the list below.

View Swift & Obj-C reference docs

View Kotlin & Java reference docs

View JS client reference docs

View all Flutter packages

View Unity reference docs

View C++ reference docs

View Admin SDK reference docs

Find detailed information about app configuration settings, or Firebase related tools.

View an index of all supported platforms, frameworks, libraries, and tools.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Structures
  - GenerateContentResponse
    - Declaration
  - Candidate
    - Declaration
  - CitationMetadata
    - Declaration
  - Citation
    - Declaration

The following structures are available globally.

The model’s response to a generate content request.

A struct representing a possible reply to a content generation prompt. Each content generation prompt may produce multiple candidate responses.

A collection of source attributions for a piece of content.

A struct describing a source attribution.

A value enumerating possible reasons for a model to terminate a content generation request.

A metadata struct containing any feedback the model had on the prompt it was provided.

Metadata returned to the client when grounding is enabled.

If using Grounding with Google Search, you are required to comply with the “Grounding with Google Search” usage requirements for your chosen API provider: Gemini Developer API or Vertex AI Gemini API (see Service Terms section within the Service Specific Terms).

Represents a specific segment within a ModelContent struct, often used to pinpoint the exact location of text or data that grounding information refers to.

A struct defining model parameters to be used when sending generative AI requests to the backend model.

Configuration parameters for sending requests to the backend.

Represents token counting info for a single modality.

Content part modality.

A type describing data in media formats interpretable by an AI model. Each generative AI request or response contains an Array of ModelContents, and each ModelContent value may comprise multiple heterogeneous Parts.

A type defining potentially harmful media categories and their model-assigned ratings. A value of this type may be assigned to a category for every model-generated response, not just responses that exceed a certain threshold.

A type used to specify a threshold for harmful content, beyond which the model will return a fallback response instead of generated content.

See safety settings for Gemini models for more details.

Categories describing the potential harm a piece of content may pose.

Structured representation of a function declaration.

This FunctionDeclaration is a representation of a block of code that can be used as a Tool by the model and executed by the client.

A tool that allows the generative model to connect to Google Search to access and incorporate up-to-date information from the web into its responses.

When using this feature, you are required to comply with the “Grounding with Google Search” usage requirements for your chosen API provider: Gemini Developer API or Vertex AI Gemini API (see Service Terms section within the Service Specific Terms).

A helper tool that the model may use when generating responses.

A Tool is a piece of code that enables the system to interact with external systems to perform an action, or set of actions, outside of knowledge and scope of the model.

Configuration for specifying function calling behavior.

Tool configuration for any Tool specified in the request.

The model’s response to a count tokens request.

Represents available backend APIs for the Firebase AI SDK.

An aspect ratio for images generated by Imagen.

To specify an aspect ratio for generated images, set aspectRatio in your ImagenGenerationConfig. See the Cloud documentation for more details and examples of the supported aspect ratios.

Configuration options for generating images with Imagen.

See Parameters for Imagen models to learn about parameters available for use with Imagen models, including how to configure them.

A response from a request to generate images with Imagen.

The type placeholder T is an image type; this is currently always an ImagenInlineImage.

This type is returned from:

An image format for images generated by Imagen.

To specify an image format for generated images, set imageFormat in your ImagenGenerationConfig. See the Cloud documentation for more details.

An error that occurs when image generation fails due to all generated images being blocked.

The images may have been blocked due to the specified ImagenSafetyFilterLevel, the ImagenPersonFilterLevel, or filtering included in the model. These filter levels may be adjusted in your ImagenSafetySettings. See the Responsible AI and usage guidelines for Imagen for more details.

An image generated by Imagen, represented as inline data.

A filter level controlling whether generation of images containing people or faces is allowed.

See the personGeneration documentation for more details.

A filter level controlling how aggressively to filter sensitive content.

Text prompts provided as inputs and images (generated or uploaded) through Imagen on Vertex AI are assessed against a list of safety filters, which include ‘harmful categories’ (for example, violence, sexual, derogatory, and toxic). This filter level controls how aggressively to filter out potentially harmful content from responses. See the safetySetting documentation and the Responsible AI and usage guidelines for more details.

Settings for controlling the aggressiveness of filtering out sensitive content.

See the Responsible AI and usage guidelines for more details.

Configuration options for audio transcriptions when communicating with a model that supports the Gemini Live API.

While there are not currently any options, this will likely change in the future. For now, just providing an instance of this struct will enable audio transcriptions for the corresponding input or output fields.

Text transcription of some audio form during a live interaction with the model.

Configuration options for live content generation.

Incremental server update generated by the model in response to client messages.

Content is generated as quickly as possible, and not in realtime. Clients may choose to buffer and play it out in realtime.

Server will not be able to service client soon.

To learn more about session limits, see the docs on Maximum session duration.

Update from the server, generated from the model in response to client messages.

Request for the client to execute the provided functionCalls.

The client should return matching FunctionResponsePart, where the functionId fields correspond to individual FunctionCallParts.

Notification for the client to cancel a previous function call from LiveServerToolCall.

The client does not need to send FunctionResponseParts for the cancelled FunctionCallParts.

The model sent a message that the SDK failed to parse.

This may indicate that the SDK version needs updating, a model is too old for the current SDK version, or that the model is just not supported.

Check the NSUnderlyingErrorKey entry in errorUserInfo for the error that caused this.

The live session was closed, because the network connection was lost.

Check the NSUnderlyingErrorKey entry in errorUserInfo for the error that caused this.

The live session was closed, but not for a reason the SDK expected.

Check the NSUnderlyingErrorKey entry in errorUserInfo for the error that caused this.

The model refused our request to setup a live session.

This can occur due to the model not supporting the requested response modalities, the project not having access to the model, the model being invalid, or some internal error.

Check the NSUnderlyingErrorKey entry in errorUserInfo for the error that caused this.

Configuration for controlling the voice of the model during conversation.

A text part containing a string value.

A data part that is provided inline in requests.

Data provided as an inline data part is encoded as base64 and included directly (inline) in the request. For large files, see FileDataPart which references content by URI instead of including the data in the request.

Only small files can be sent as inline data because of limits on total request sizes; see input files and requirements for more details and size limits.

File data stored in Cloud Storage for Firebase, referenced by URI.

A predicted function call returned from the model.

Result output from a function call.

Contains a string representing the FunctionDeclaration.name and a structured JSON object containing any output from the function is used as context to the model. This should contain the result of a FunctionCallPart made based on model prediction.

A part containing code that was executed by the model.

The result of executing code.

Represents the different types, or modalities, of data that a model can produce as output.

To configure the desired output modalities for model requests, set the responseModalities parameter when initializing a GenerationConfig. See the multimodal responses documentation for more details.

Support for each response modality, or combination of modalities, depends on the model.

Configuration for controlling the “thinking” behavior of compatible Gemini models.

Certain models, like Gemini 2.5 Flash and Pro, utilize a thinking process before generating a response. This allows them to reason through complex problems and plan a more coherent and accurate answer.

A tool that allows the model to execute code.

This tool can be used to solve complex problems, for example, by generating and executing Python code to solve a math problem.

Metadata related to the urlContext() tool.

URL context is a Public Preview feature, which means that it is not subject to any SLA or deprecation policy and could change in backwards-incompatible ways.

Metadata for a single URL retrieved by the urlContext() tool.

URL context is a Public Preview feature, which means that it is not subject to any SLA or deprecation policy and could change in backwards-incompatible ways.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/GenerateContentResponse/UsageMetadata

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- UsageMetadata
  - promptTokenCount
    - Declaration
  - candidatesTokenCount
    - Declaration
  - toolUsePromptTokenCount
    - Declaration
  - thoughtsTokenCount
    - Declaration

Token usage metadata for processing the generate content request.

The number of tokens in the request prompt.

The total number of tokens across the generated response candidates.

The number of tokens used by tools.

The number of tokens used by the model’s internal “thinking” process.

For models that support thinking (like Gemini 2.5 Pro and Flash), this represents the actual number of tokens consumed for reasoning before the model generated a response. For models that do not support thinking, this value will be 0.

When thinking is used, this count will be less than or equal to the thinkingBudget set in the ThinkingConfig.

The total number of tokens in both the request and response.

The breakdown, by modality, of how many tokens are consumed by the prompt.

The breakdown, by modality, of how many tokens are consumed by the candidates

The breakdown, by modality, of how many tokens were consumed by the tools used to process the request.

---

## HostAndPort interface Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/emulator-suite/rules-unit-testing/rules-unit-testing.hostandport

**Contents:**
- HostAndPort interface Stay organized with collections Save and categorize content based on your preferences.
- Properties
- HostAndPort.host
- HostAndPort.port

The host of the emulator. Can be omitted if discovered automatically through the hub or specified via environment variables. See TestEnvironmentConfig for details.

The port of the emulator. Can be omitted if discovered automatically through the hub or specified via environment variables. See TestEnvironmentConfig for details.

**Examples:**

Example 1 (typescript):
```typescript
export interface HostAndPort
```

Example 2 (yaml):
```yaml
host: string;
```

Example 3 (yaml):
```yaml
port: number;
```

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/CitationMetadata

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- CitationMetadata
  - citations
    - Declaration
- Codable Conformances
  - init(from:)
    - Declaration

A collection of source attributions for a piece of content.

A list of individual cited sources and the parts of the content to which they apply.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/Citation

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Citation
  - startIndex
    - Declaration
  - endIndex
    - Declaration
  - uri
    - Declaration
  - title
    - Declaration

A struct describing a source attribution.

The inclusive beginning of a sequence in a model response that derives from a cited source.

The exclusive end of a sequence in a model response that derives from a cited source.

A link to the cited source, if available.

The title of the cited source, if available.

The license the cited source work is distributed under, if specified.

The publication date of the cited source, if available.

DateComponents can be converted to a Date using the date computed property.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Extensions/CIImage

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- CIImage
  - partsValue
    - Declaration

Enables CIImages to be representable as model content.

---

## Supporting iOS 14 Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/ios/supporting-ios-14

**Contents:**
- Supporting iOS 14 Stay organized with collections Save and categorize content based on your preferences.
- Affected Firebase products
- Affected Firebase integrations
- Requesting App Tracking Permission on iOS 14
  - Add In-App Messaging to your app
  - Handle in-app message dismissal
  - Swift
  - Swift
  - Create an In-App Messaging campaign
- Optional: A/B Test different explainer screens

With iOS 14.5, Apple requires developers to receive the user’s permission through the App Tracking Transparency framework to track them or access their device’s advertising identifier (IDFA). See Apple's User Privacy and Data Use and Apple's App Tracking Transparency documentation for more details.

Firebase SDKs do not access IDFA, though some have integrations with Google Analytics that may involve IDFA access.

The table below lists Firebase products that are available on Apple platforms and describes how the functionality of each product is impacted if IDFA is not accessible.

1 Firebase AI Logic was formerly called "Vertex AI in Firebase".

The table below lists Firebase-integrated products that are affected if IDFA is not accessible.

If you would like your Apple application to be able to access IDFA, you can add Apple's App Tracking Transparency framework to your app and request permission to track or access your users' IDFA.

Many applications choose to present a warm-up, or explainer, screen prior to asking for permission. The explainer screen allows you to give users more context on how your app uses IDFA prior to requesting access.

If you are an AdMob or Ad Manager app publisher, consider using Funding Choices, which handles obtaining consent for serving personalized advertisements as well as consent for tracking the user according to Apple's guidelines automatically. See the AdMob Consent with User Messaging page for more details.

The following guide provides a solution using Firebase In-App Messaging for creating and displaying an explainer screen prior to requesting tracking access via App Tracking Transparency.

Follow the instructions to add In-App Messaging to your Apple application.

First, avoid displaying the explainer screen on devices that cannot present the consent dialog, such as devices running iOS 13. Make sure this code executes immediately after FirebaseApp.configure().

Implement the InAppMessagingDisplayDelegate protocol to handle events when the user dismisses the explainer screen. If the user taps OK, display the system prompt via the App Tracking Transparency framework.

Once the code is in place in your application, create an in-app message in the Firebase console.

You can customize the appearance of the explainer screen by following the instructions in the In-App Messaging documentation.

In-App Messaging has a built-in integration with Firebase A/B Testing, which you can use to experiment with different explainer screens.

Firebase A/B Testing automatically creates experiment groups and helps you visualize how users interact with different variants of your application.

If you didn't log a Google Analytics event when handling the app tracking permissions response, you will need to in order to measure changes in the response rate when running an A/B experiment.

In the Analytics section of the Firebase console, navigate to the Conversions menu, then add a new conversion event with the same name as the event logged with the sample code above.

In the console's In-App Messaging menu, click New Experiment, then follow the instructions on the resulting screens.

Once you've published your experiment, it will need to collect data for some time before it can produce conclusive results.

Read the Firebase A/B Testing documentation for information on how to monitor an experiment and roll out a successful variant.

**Examples:**

Example 1 (unknown):
```unknown
if NSClassFromString("ATTrackingManager") == nil {
  // Avoid showing the App Tracking Transparency explainer if the
  // framework is not linked.
  InAppMessaging.inAppMessaging().messageDisplaySuppressed = true
}
```

Example 2 (go):
```go
// The InAppMessaging delegate must be assigned before events can be handled.
InAppMessaging.inAppMessaging().delegate = self

func messageClicked(_ inAppMessage: InAppMessagingDisplayMessage,
                    with action: InAppMessagingAction) {
  switch action.actionText {
  case "OK":
    ATTrackingManager.requestTrackingAuthorization { status in
      switch status {
      case .authorized:
        // Optionally, log an event when the user accepts.
        Analytics.logEvent("tracking_authorized", parameters: nil)
      case _:
        // Optionally, log an event here with the rejected value.
      }
    }
  case _:
    // do nothing
  }
}
```

Example 3 (css):
```css
ATTrackingManager.requestTrackingAuthorization { status in
  switch status {
  case .authorized:
    // Optionally, log an event when the user accepts.
    Analytics.logEvent("tracking_authorized", parameters: nil)
  case _:
    // Optionally, log an event here with the rejected value.
  }
}
```

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Enums/JSONValue

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- JSONValue
  - null
    - Declaration
  - number(_:)
    - Declaration
  - string(_:)
    - Declaration
  - bool(_:)
    - Declaration

Represents a value in one of JSON’s data types.

This may be decoded from, or encoded to, a google.protobuf.Value.

An array of JSONValues.

---

## FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebasecore/api/reference/Enums

**Contents:**
- FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Enumerations
  - FIRLoggerLevel

The following enumerations are available globally.

The log levels used by internal logging.

---

## 

**URL:** https://firebase.google.com/docs/reference/unity

**Contents:**
- Firebase Unity API Reference
- Firebase
  - Classes
- Firebase.AI
  - Classes
  - Interfaces
  - Structs
- Firebase.Analytics
  - Classes
- Firebase.AppCheck

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/FunctionResponsePart

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- FunctionResponsePart
  - functionId
    - Declaration
  - name
    - Declaration
  - response
    - Declaration
  - isThought
    - Declaration

Result output from a function call.

Contains a string representing the FunctionDeclaration.name and a structured JSON object containing any output from the function is used as context to the model. This should contain the result of a FunctionCallPart made based on model prediction.

Matching functionId for a FunctionCallPart, if one was provided.

The name of the function that was called.

The function’s response or return value.

Constructs a new FunctionResponse.

The name of the function that was called.

The function’s response.

Constructs a new FunctionResponse.

The name of the function that was called.

The function’s response.

Matching functionId for a FunctionCallPart, if one was provided.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes/Schema/StringFormat

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- StringFormat
  - custom(_:)
    - Declaration

Modifiers describing the expected format of a string Schema.

A custom string format.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/GroundingMetadata/WebGroundingChunk

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- WebGroundingChunk
  - uri
    - Declaration
  - title
    - Declaration
  - domain
    - Declaration

A grounding chunk sourced from the web.

The URI of the retrieved web page.

The title of the retrieved web page.

The domain of the original URI from which the content was retrieved.

This field is only populated when using the Vertex AI Gemini API.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Extensions/String

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- String
  - partsValue
    - Declaration

Enables a String to be passed in as PartsRepresentable.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/FunctionDeclaration

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- FunctionDeclaration
  - init(name:description:parameters:optionalParameters:)
    - Declaration
    - Parameters
- Codable Conformance
  - encode(to:)
    - Declaration

Structured representation of a function declaration.

This FunctionDeclaration is a representation of a block of code that can be used as a Tool by the model and executed by the client.

Constructs a new FunctionDeclaration.

The name of the function; must be a-z, A-Z, 0-9, or contain underscores and dashes, with a maximum length of 63.

A brief description of the function.

Describes the parameters to this function.

The names of parameters that may be omitted by the model in function calls; by default, all parameters are considered required.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/HarmCategory

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- HarmCategory
  - harassment
    - Declaration
  - hateSpeech
    - Declaration
  - sexuallyExplicit
    - Declaration
  - dangerousContent
    - Declaration

Categories describing the potential harm a piece of content may pose.

Negative or harmful comments targeting identity and/or protected attributes.

Contains references to sexual acts or other lewd content.

Promotes or enables access to harmful goods, services, or activities.

Content that may be used to harm civic integrity.

Returns the raw string representation of the HarmCategory value.

This value directly corresponds to the values in the REST API.

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Classes/Schema/IntegerFormat

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- IntegerFormat
  - int32
    - Declaration
  - int64
    - Declaration
  - custom(_:)
    - Declaration

Modifiers describing the expected format of an integer Schema.

A 32-bit signed integer.

A 64-bit signed integer.

A custom integer format.

---

## Admin SDK Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/admin

**Contents:**
- Admin SDK Reference Stay organized with collections Save and categorize content based on your preferences.
- Node.js
- Java
- Python
- Go
- C# (.NET)

The Admin SDK is a set of server libraries that lets you interact with Firebase from privileged environments. The SDK supports Node.js, Java, Python, Go, and C# (.NET). For more information about feature support and setup tasks, see Add the Firebase Admin SDK to Your Server.

The Admin SDK for Node.js provides APIs for authentication, user management, Realtime Database, and more.

The Admin SDK for Java provides APIs for authentication, user management, Realtime Database, and more.

The Admin SDK for Python provides APIs for authentication, user management, Realtime Database, and more.

The Admin SDK for Go provides APIs for authentication, user management, Realtime Database, and more.

The Admin SDK for .NET provides APIs for authentication (ID token verification and custom token minting).

---

## Cloud Firestore Index Definition Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/firestore/indexes

**Contents:**
- Cloud Firestore Index Definition Reference Stay organized with collections Save and categorize content based on your preferences.
- Deploy an index configuration
- JSON format
  - Indexes
    - Standard edition
    - Enterprise edition
  - FieldOverrides
    - TTL Policy

Cloud Firestore automatically creates indexes to support the most common types of queries, but allows you to define custom indexes and index overrides as described in the Cloud Firestore guides.

You can create, modify and deploy custom indexes in the Firebase console, or using the CLI. From the CLI, edit your index configuration file, with default filename firestore.indexes.json, and deploy using the firebase deploy command.

You can export indexes with the CLI using firebase firestore:indexes.

An index configuration file defines one object containing an indexes array and an optional fieldOverrides array. Here's an example:

Deploy your index configuration with the firebase deploy command. If you only want to deploy indexes for the databases configured in your project, add the --only firestore flag. See the options reference for this command.

To list deployed indexes, run the firebase firestore:indexes command. Add the --database=<databaseID> flag to list indexes for a database other than your project's default database.

If you make edits to the indexes using the Firebase console, make sure you also update your local indexes file. For more on managing indexes, see the Cloud Firestore guides.

The schema for one object in the indexes array is as follows. Optional properties are identified with the ? character.

Note that Cloud Firestore document fields can only be indexed in one mode, thus a field object can only contain one of the order, arrayConfig, and vectorConfig properties.

While there are fields and properties that are specific to Cloud Firestore editions, some fields are shared as well.

The schema for one object in the fieldOverrides array is as follows. Optional properties are identified with the ? character.

Note that Cloud Firestore document fields can only be indexed in one mode, thus a field object cannot contain both the order and arrayConfig properties.

A TTL policy can be enabled or disabled using the fieldOverrides array as follows:

To keep the default indexing in the field and enable a TTL policy:

For more information about time-to-live (TTL) policies review the official documentation.

**Examples:**

Example 1 (json):
```json
{
  // Required, specify compound and vector indexes
  indexes: [
    {
      collectionGroup: "posts",
      queryScope: "COLLECTION",
      fields: [
        { fieldPath: "author", arrayConfig: "CONTAINS" },
        { fieldPath: "timestamp", order: "DESCENDING" }
      ]
    },
    {
      collectionGroup: "coffee-beans",
      queryScope: "COLLECTION",
      fields: [
        {
          fieldPath: "embedding_field",
          vectorConfig: { dimension: 256, flat: {} }
        }
      ]
    }
  ],

  // Optional, disable indexes or enable single-field collection group indexes
  fieldOverrides: [
    {
      collectionGroup: "posts",
      fieldPath: "myBigMapField",
      // We want to disable indexing on our big map field, and so empty the indexes array
      indexes: []
    }
  ]
}
```

Example 2 (json):
```json
collectionGroup: string  // Labeled "Collection ID" in the Firebase console
  queryScope: string       // One of "COLLECTION", "COLLECTION_GROUP"
  apiScope: string         // "ANY_API" (the default) is the only acceptable value. Optional.
  density: string          // "SPARSE_ALL" is the only acceptable value. Optional.
  fields: array
    fieldPath: string
    order?: string         // One of "ASCENDING", "DESCENDING"; excludes arrayConfig and vectorConfig properties
    arrayConfig?: string   // If this parameter used, must be "CONTAINS"; excludes order and vectorConfig properties
    vectorConfig?: object  // Indicates that this is a vector index; excludes order and arrayConfig properties
      dimension: number    // The resulting index will only include vectors of this dimension
      flat: {}             // Indicates the vector index is a flat index
```

Example 3 (json):
```json
collectionGroup: string  // Labeled "Collection ID" in the Firebase console
  queryScope: string       // One of "COLLECTION", "COLLECTION_GROUP"
  apiScope: string         // "MONGODB_COMPATIBLE_API" is the only acceptable value
  multikey: boolean        // Indicates if this is a multikey index
  density: string          // One of "SPARSE_ANY" or "DENSE"
  fields: array
    fieldPath: string
    order?: string         // One of "ASCENDING", "DESCENDING"; excludes arrayConfig and vectorConfig properties
    arrayConfig?: string   // If this parameter used, must be "CONTAINS"; excludes order and vectorConfig properties
    vectorConfig?: object  // Indicates that this is a vector index; excludes order and arrayConfig properties
      dimension: number    // The resulting index will only include vectors of this dimension
      flat: {}             // Indicates the vector index is a flat index
```

Example 4 (yaml):
```yaml
collectionGroup: string  // Labeled "Collection ID" in the Firebase console
  fieldPath: string
  ttl?: boolean            // Set specified field to have TTL policy and be eligible for deletion
  indexes: array           // Use an empty array to disable indexes on this collectionGroup + fieldPath
    queryScope: string     // One of "COLLECTION", "COLLECTION_GROUP"
    order?: string         // One of "ASCENDING", "DESCENDING"; excludes arrayConfig property
    arrayConfig?: string   // If this parameter used, must be "CONTAINS"; excludes order property
```

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/ImagenImageFormat

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- ImagenImageFormat
  - png()
    - Declaration
  - jpeg(compressionQuality:)
    - Declaration
    - Parameters

An image format for images generated by Imagen.

To specify an image format for generated images, set imageFormat in your ImagenGenerationConfig. See the Cloud documentation for more details.

Portable Network Graphic (PNG) is a lossless image format, meaning no image data is lost during compression. Images in PNG format are typically larger than JPEG images, though this depends on the image content and JPEG compression quality.

Joint Photographic Experts Group (JPEG) is a lossy compression format, meaning some image data is discarded during compression. Images in JPEG format are typically larger than PNG images, though this depends on the image content and JPEG compression quality.

The JPEG quality setting from 0 to 100, where 0 is highest level of compression (lowest image quality, smallest file size) and 100 is the lowest level of compression (highest image quality, largest file size); defaults to 75.

---

## 

**URL:** https://firebase.google.com/docs/reference/cpp

**Contents:**
- Firebase C++ API Reference
- firebase
  - Classes
- firebase::analytics
  - Structs
- firebase::app_check
  - Classes
  - Structs
- firebase::auth
  - Classes

---

## FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebaseailogic/api/reference/Structs/ExecutableCodePart/Language

**Contents:**
- FirebaseAILogic Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- Language
  - python
    - Declaration
  - description
    - Declaration

The language of the code in an ExecutableCodePart.

The Python programming language.

---

## FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.

**URL:** https://firebase.google.com/docs/reference/swift/firebasecore/api/reference/Classes/FirebaseOptions

**Contents:**
- FirebaseCore Framework Reference Stay organized with collections Save and categorize content based on your preferences.
- FirebaseOptions
  - defaultOptions()
    - Declaration
  - apiKey
    - Declaration
  - bundleID
    - Declaration
  - clientID
    - Declaration

This class provides constant fields of Google APIs.

Returns the default options. The first time this is called it synchronously reads GoogleService-Info.plist from disk.

An API key used for authenticating requests from your Apple app, e.g. The key must begin with “A” and contain exactly 39 alphanumeric characters, used to identify your app to Google servers.

The bundle ID for the application. Defaults to Bundle.main.bundleIdentifier when not set manually or in a plist.

The OAuth2 client ID for Apple applications used to authenticate Google users, for example @“12345.apps.googleusercontent.com”, used for signing in with Google.

The Project Number from the Google Developer’s console, for example @“012345678901”, used to configure Firebase Cloud Messaging.

The Project ID from the Firebase console, for example @“abc-xyz-123”.

The Google App ID that is used to uniquely identify an instance of an app.

The database root URL, e.g. @“http://abc-xyz-123.firebaseio.com”.

The Google Cloud Storage bucket name, e.g. @“abc-xyz-123.storage.firebase.com”.

The App Group identifier to share data between the application and the application extensions. The App Group must be configured in the application and on the Apple Developer Portal. Default value nil.

Initializes a customized instance of FirebaseOptions from the file at the given plist file path. This will read the file synchronously from disk. For example:

Note that it is not possible to customize FirebaseOptions for Firebase Analytics which expects a static file named GoogleService-Info.plist - https://github.com/firebase/firebase-ios-sdk/issues/230. Returns nil if the plist file does not exist or is invalid.

Initializes a customized instance of FirebaseOptions with required fields. Use the mutable properties to modify fields for configuring specific services. Note that it is not possible to customize FirebaseOptions for Firebase Analytics which expects a static file named GoogleServices-Info.plist - https://github.com/firebase/firebase-ios-sdk/issues/230.

Unavailable. Please use init(contentsOfFile:) or init(googleAppID:gcmSenderID:) instead.

---

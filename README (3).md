<p align="center"><img src="banner.png" alt="Jo, a voice assistant for Android" width="100%"></p>

<p align="center">
  <b>Say "Hey Jo". Jo does the rest.</b><br>
  An Android voice assistant that wakes offline, answers only to you, and can operate your apps like a pair of hands.
</p>

<p align="center">
  <img alt="Android" src="https://img.shields.io/badge/Android-8.0%2B-3DDC84?logo=android&logoColor=white">
  <img alt="Kotlin" src="https://img.shields.io/badge/Kotlin-7F52FF?logo=kotlin&logoColor=white">
  <img alt="Offline wake word" src="https://img.shields.io/badge/wake%20word-offline-06101F">
  <img alt="Build" src="https://img.shields.io/badge/build-GitHub%20Actions-2088FF?logo=githubactions&logoColor=white">
</p>

<p align="center"><img src="demo.gif" alt="Jo demo" width="280"></p>

## Features

- **Wake word, offline.** Say "Hey / Hello / Hi Jo". Listening runs on-device with [Vosk](https://alphacephei.com/vosk/); nothing is streamed while Jo rests.
- **Owner-only voice.** Jo learns your voice (speaker embeddings) and ignores everyone else.
- **Real-time commands.** Call, text, open apps, timers, alarms, reminders, media keys, battery, navigation, and more.
- **Smart mode (optional).** Plug in a free-tier LLM key (Gemini, Groq, OpenRouter, Cerebras, Mistral, GitHub Models, Anthropic or any custom HTTPS endpoint) so Jo understands natural, multi-step requests.
- **Screen control (optional).** Through Android Accessibility, Jo reads the screen, taps, types and scrolls. Example: *"Open Instagram and scroll my reels."*
- **Short replies.** Jo says as little as possible. A "Speak replies" switch makes it fully silent.
- **Black and silver UI.** Minimal, glass-style, no clutter.

## How it works

```
mic -> wake word (Vosk) -> owner check (voice print) -> command
        |                                                  |
        |                              built-in rules  <---+---> LLM (optional)
        |                                      |                    |
        +--- foreground service                +---- whitelisted actions
                                               +---- screen agent (observe -> act -> repeat)
```

The model only *proposes* actions. Jo's own rules decide what is allowed to run.

## Security and privacy

- Wake-word listening and owner verification are **on-device**.
- API key is stored **encrypted** with the Android Keystore (AES-GCM) and never logged.
- **Rate limits:** 6 calls/min, 1.5 s spacing, circuit breaker after 3 failures, daily caps (brain calls and screen steps).
- HTTPS only, no cleartext traffic, system certificates only, no redirects followed. Backups disabled.
- AI output can only trigger a **whitelist** of commands (max 4 per reply). It cannot confirm its own actions.
- **Confirmations:** payments and deletions always need a spoken "yes"; send/post/share need "yes" unless you enable auto-send.
- **Blocked apps:** banking, UPI, wallets, password managers, authenticators, the Play Store and Jo itself.
- Password fields are never read or typed. Text on screen is treated as data, never as instructions.
- With Smart mode on, your spoken command (and, during screen control, a trimmed view of the current screen) is sent to the provider you chose. Turn both off and nothing leaves the phone.

## Build (no computer needed)

1. Create a GitHub repo and upload `jo-src.zip` to its root.
2. Add `.github/workflows/build.yml` (the workflow in this repo). It unpacks the zip, downloads the voice models, runs unit tests and builds the APK.
3. Open **Actions**, run the latest workflow, and download the **jo-apk** artifact.
4. Install the APK. Updates install over older versions (fixed signing key).

If a build fails, the error list appears in the run's summary.

## First run

1. Allow microphone, contacts and calls.
2. Allow "display over other apps".
3. Turn on **Jo** in Accessibility.
4. Disable battery optimisation for Jo so it keeps listening.
5. Teach Jo your voice.
6. Optional: open Settings, pick a provider, tap **Get free key**, paste it, tap **Test**. Then switch on **Let Jo control apps**.

## Try saying

| Say | Jo does |
|---|---|
| "Hey Jo, call Mom" | Calls your contact |
| "Text Prem I'm on my way" | Sends an SMS after confirming |
| "Set a timer for ten minutes" | Starts a timer |
| "Remind me at six to buy milk" | Sets a reminder |
| "Pause" / "Next song" | Media keys |
| "Navigate to the airport" | Opens turn-by-turn navigation |
| "Open Instagram and scroll reels" | Screen control (needs Smart mode) |
| "Stop" | Ends the current task |

## Limitations

- Pre-release software. Verify behavior on your own device before relying on it.
- Some apps block accessibility access; Jo cannot operate those.
- Free-tier models, limits and names change. Use the provider list in Settings.
- Screen control is only as reliable as the model driving it and the app's layout.

## Project layout

```
app/src/main/java/com/jp/   Kotlin sources (service, commands, brain, screen agent)
app/src/main/assets/ui/     WebView UI (index.html)
app/src/test/               Unit tests for the pure logic
.github/workflows/          CI build
```

## Roadmap

- Better multi-language wake and commands
- More built-in commands
- Per-app screen-control recipes

## Acknowledgements

[Vosk](https://alphacephei.com/vosk/) for offline speech recognition.

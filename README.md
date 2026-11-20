# AI Video Clipping Bot

An open-source Python MVP that turns an authorized long-form video into short, vertical clips and returns them through Telegram.

> **Status:** Early development. The workflow below describes the intended MVP; integrations must be tested before being described as production-ready.

## How it works

1. **Submit a video** — A user sends a supported video URL to the Telegram bot.
2. **Validate and download** — The bot validates the URL and uses `yt-dlp` to retrieve a video the user owns or is authorized to process.
3. **Inspect media** — `ffprobe` reads the media duration and metadata. FFmpeg extracts a mono audio track for transcription.
4. **Transcribe speech** — Whisper produces timestamped transcript segments.
5. **Find highlights** — The MVP selects short, self-contained moments from the transcript. An AI-based selector can rank candidate moments; a basic heuristic can serve as a fallback.
6. **Render vertical clips** — FFmpeg cuts each selected time range, scales and crops it to a 9:16 frame, and encodes an MP4.
7. **Add captions** — Transcript segments are converted into timed subtitles and can be burned into the rendered clip.
8. **Return results** — The Telegram bot sends the finished MP4 files back to the user.

## Planned technology

| Component | Technology | Responsibility |
|---|---|---|
| Application | Python | Orchestration and bot logic |
| Chat interface | Telegram Bot API | URL input, status updates, clip delivery |
| Video retrieval | yt-dlp | Download supported, authorized source videos |
| Media inspection | ffprobe | Read duration and media metadata |
| Media processing | FFmpeg | Audio extraction, trimming, vertical rendering, subtitles |
| Transcription | Whisper / faster-whisper | Timestamped speech-to-text |
| Highlight selection | AI model + fallback heuristic | Select candidate moments |
| Optional audio features | ElevenLabs | Future voice generation or supported audio enhancement |

ElevenLabs is optional and is **not required** for the initial clipping pipeline. It should only be integrated if a specific audio feature is agreed.

## MVP scope

- Telegram bot accepts a video URL.
- Download and process a supported video.
- Generate a timestamped transcript.
- Select up to a configurable number of highlights.
- Render 9:16 MP4 clips.
- Generate and optionally burn subtitles.
- Send clips back through Telegram.
- Report understandable errors when a URL or processing step fails.

## Out of scope for the first MVP

- Face tracking and automatic speaker reframing.
- Multi-user billing or subscriptions.
- A production job queue and distributed workers.
- Persistent user accounts or a database.
- Guaranteed support for every video platform.
- Advanced editing templates or fully animated captions.

## Development milestones

1. Initialize the repository and document the architecture.
2. Implement and test the media-processing pipeline locally.
3. Connect the pipeline to a Telegram bot.
4. Test with a video the developer owns or is authorized to process.
5. Record a demonstration and document known limitations.

## Responsible use

Only process videos that you own or have permission to download and edit. Respect copyright, privacy, platform terms, and access restrictions. This project does not attempt to bypass DRM, authentication, or other access controls.

## License

MIT License (planned; add a `LICENSE` file before distributing releases).

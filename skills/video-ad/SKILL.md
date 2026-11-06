---
name: video-ad
description: Produce short video ads with Magnific - product commercials, social spots for Reels, TikTok and Shorts, UGC-style talking videos, and TV-style 15/30-second cuts - from a brief, a packshot or a key visual, with the product kept exact, music or voiceover, and every format the media plan needs. Use it whenever someone wants an ad, spot, commercial, promo, product video, UGC video or social video made with Magnific, even if they only say "animate this product" or "make a reel".
---

# Video ads

Video ads are one of the most common jobs people bring to Magnific through AI assistants.
The professional bar is simple to state and easy to miss: the real product stays exact in every frame, the cut lands on time with sound, and the client sees the cost before the expensive renders.

Follow `magnific-essentials` for pricing, uploads, project folders, waiting and delivery.
The video prompt craft is in [video-prompt-craft.md](../magnific-essentials/references/video-prompt-craft.md); read it before writing the first `video_generate` prompt.

## 1. Lock the brief in one pass

Settle these from the request, and state the ones you had to assume:

- Platform and format: Reels, TikTok, Shorts and Stories are 9:16; YouTube and web are 16:9; feed is 1:1. No video model renders 4:5; make 1:1 or 9:16 and reframe later if needed.
- Length and shot count: 6, 10, 15 or 30 seconds; roughly one shot per 3 to 5 seconds.
- Product source: a packshot creation, a saved library product (`library_list`), or the user's file to upload.
- Sound: music, voiceover, on-camera dialogue, sound effects, or silence.
- Budget when the user stated a spending cap, and the project to save into.
- The end card: where the logo and the call to action go. Leave clean negative space for them; let the editor add exact type and logos, because generated text and logos are unreliable.
  In 9:16 for Reels, TikTok and Shorts, put that space in the upper or middle third: the platform's captions and buttons cover the bottom fifth of the frame.

## 2. Write a shot plan before you spend

A numbered table: shot, seconds, framing and camera move, action, product moment, sound.
Make the first 1 to 2 seconds a hook (motion, reveal or a question), the middle the proof (product in use, texture, benefit), and the last shot the payoff on the product with space for the end card.
A 15-second three-shot luxury spot, for example: macro texture reveal 0-5 s, product in hand or in use 5-10 s, hero product on a surface with slow push-in and negative space 10-15 s.

## 3. Keep the product exact

The product drifting (wrong cap, invented label text, changed proportions) is the failure clients reject first.
Pick one of these wirings per shot:

- **Start frame route (most control):** generate a still for each shot with `images_generate`, passing the packshot as the first `image` reference (or the library product as `{type: "product", identifier: <id>}`), with a lock-list prompt ("the exact product from the reference, same shape, cap, label and colors"). Approve the stills, then animate each with `keyframes.start`.
- **Reference route (fewer steps):** `video_generate` text-to-video with the product in `references[]`: a creation as `{type: "image", url: <identifier>}`, or a library product as `{type: "product", custom_model_id: "<id>", name: "<name>"}`.

Seedance 2.x and MiniMax H3 cannot take a start frame and visual references in the same call, so never send both there.
Inside any move that turns the product, add a guard: "during the move the label, cap and proportions stay exactly as in the product reference."

## 4. Choose quality, then render

- Follow `magnific-essentials` for model and resolution priority, explicit choices and capability fallbacks.
  Prefer quality using the live catalog’s `agentRecommendation`, including for Reels, TikTok and Shorts.
  Choose a compatible public model and supported resolution for the requested deliverable.
  A social placement or a new brief alone does not justify a cheaper model or lower resolution.
- Use cheaper models or lower resolution for an explicit budget or speed priority, or when the user requests drafts or previews.
  For a draft workflow, choose a compatible recommended model from the live catalog and review motion and product fidelity before paying for finals.
- Price that plan with `simulate_cost` (one shot, times the number of shots) before rendering.
- Seedance 2.5 `Draft` previews the exact take; `video_finalize_draft` renders that take at 1080p, billed like a 1080p render.
  Draft plus finalize costs more than a direct final. Use it when someone will review drafts before selecting finals.
- With a budget, leave room to fix the worst shot. Compare live quotes for compatible alternatives if the preferred route exceeds that budget.
- With a budget, run drafts and finals inside it without stopping. Without one, follow the spend rule in `magnific-essentials`: quote the plan and wait for a yes before spending more than about 200 credits.
- Start at most a few video jobs at a time; the account has a concurrency cap, and a rejected call tells you how long to wait. Never fire the same paid call twice; wait for the first.

Seedance 2.5 can render up to 6 shots and 30 seconds in one call with `multi_prompt`, which keeps the product, voice and sound consistent across cuts.
Use it for a whole short spot when you are not using start frames.
The trade-off is control: one bad shot means re-rendering the whole spot. When each shot starts from an approved still, or the budget has no room for a full re-render, render the shots separately and join them.

## 5. Sound

- Music only: generate picture without music (`noMusic: true` keeps sound effects), join the shots, then score the joined cut with `video_music` (it fits the music to the cut), or make a bed with `audio_music_generate` and lay it with `video_audio_mix`.
- Voiceover: write it to time (about 2.5 words per second), generate with `audio_tts` after `audio_tts_direction`, and mix it over the cut with `video_audio_mix`; duck music under it.
- On-camera speech (UGC, testimonial, presenter): use a native-audio model and write the line in quotes after the speaker, with delivery. Use `audio_tts` plus `video_speak` only when a specific voice must be exact or repeat across clips.
See the `sound-and-voice` skill for voice choice and mixing.

## 6. Assemble and deliver

- `video_concatenate` the completed shots in order (it keeps each clip's own audio), then add the continuous music or VO bed.
- Other aspect ratios from the master: reframe the approved cut rather than regenerating it, with the reframe route in `video-finishing`.
  When every format matters from the start, plan shots that keep the action in the center so a crop to 9:16 does not lose it.
- Deliver `h264` for social upload and review, `prores_422hq` when an editor will finish it.
- For client approval, follow the review rule in `magnific-essentials`.

## UGC-style videos

They should look like a phone video, not an ad: first-person or selfie framing, handheld micro-drift, natural window light, real room, one tactile action per beat, the line delivered casually in quotes.
Keep the body logic possible: in a selfie one hand holds the phone, so she has one free hand for the product; no mirror unless the shot is a mirror selfie.
Use an invented person, never a real or famous one, and never imitate a specific creator.
Seedance blocks realistic human faces in uploaded photos, so a UGC presenter comes from the prompt or an AI-generated character, not a photo of a real person.
Start the spoken line within the first second (that is the hook) and let it fill most of the clip, with only a short settle at the end.
Cast cheaply before the expensive render: a couple of start-frame stills with the product reference, checked for age, look and label, then one render from the chosen still.
Offer variants that test different hooks (first 2 seconds) rather than different products or looks.

## Report

Tell the user: the cut and its formats with links, which model and resolution, the credits spent against the budget, what you assumed, and what you would do next (for example, end-card type in the edit, a 1:1 reframe, hook variants).

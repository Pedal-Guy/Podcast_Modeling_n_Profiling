# Raw research D — NAM (Neural Amp Modeler), NAM A2 (Full / Lite), TONE3000
Research date: 2026-09-27. Source: research sub-agent report (English). [UNVERIFIED] marked.

## 1. NAM origin
- Steven Atkinson, PhD, applied scientist (ResearchGate: "Applied Scientist at Amazon"; Bayesian inference/ML research with GE Global Research & Notre Dame co-authors; PhD university [UNVERIFIED]). Now "Atkinson Advanced Modeling, LLC"; full-time on NAM from 9 May 2025. https://www.researchgate.net/profile/Steven-Atkinson-2 ; https://www.neuralampmodeler.com/post/i-m-now-working-on-nam-full-time
- "The History of NAM": began 2019: "As a guitarist who was also a scientist at my day job and getting into machine learning for science applications it seemed like a nice for-fun project to see whether I could apply those skills to my musical hobby." Worked "closed-book", "a crossword puzzle". https://www.neuralampmodeler.com/post/the-history-of-nam
- TONE3000 interview 17 Nov 2025: "if you love how your amp chugs, you literally record yourself chugging, and now that sound is yours forever." "If I stopped working on it tomorrow, the code is out there." https://www.tone3000.com/blog/ai-guitar-tone-interview-neural-amp-modeler-inventor-steven-atkinson
- v0.1.0 "Initial version based on TF1": 21 Feb 2020. https://github.com/sdatkinson/neural-amp-modeler/releases/tag/v0.1.0
- "Second wind" Feb 2022: iPlug2 plugin + comparison videos; claims more accurate than Kemper (early 2022), QC (May 2022), TONEX (Aug 2022).
- Universal snapshot loader Nov 2022.
- Popularity early 2023: FB group from "a couple hundred to thousands" late Feb 2023, >15,000 by end 2023. Oli Larkin (iPlug2 author) reworked plugin, added EQ + IR loader; current look Apr 2023. Bedroom Producers Blog 14 Mar 2023. https://bedroomproducersblog.com/2023/03/14/neural-amp-modeler/ . Kemper forum 25 Mar 2023 legality question. https://forum.kemper-amps.com/forum/thread/59708-neural-amp-modeler-a-free-amp-profiler/
- Components (MIT): Trainer sdatkinson/neural-amp-modeler (Python/PyTorch; Colab or local GUI); Plugin NeuralAmpModelerPlugin (iPlug2; VST3/AU/standalone; now distributed as "Gateway" — KVR "Gateway 1.0.1… Load and play snapshot models", rename date [UNVERIFIED]); Core NeuralAmpModelerCore (C++). https://github.com/sdatkinson/NeuralAmpModelerPlugin ; https://www.kvraudio.com/product/gateway-by-steven-atkinson-atkinson-advanced-modeling
- .nam format: JSON with version, architecture (WaveNet, LSTM, Sequential…), config, weights; optional sample_rate (default 48 kHz); metadata (name, modeled_by, gear make/model, gear_type amp/pedal/amp_cab…, tone_type clean/crunch/hi_gain/fuzz…, input_level_dbu/output_level_dbu — for input calibration; added format v0.5.4 w/ trainer v0.10.0). https://neural-amp-modeler.readthedocs.io/en/latest/model-file.html
- Workflow: play standard test signal (input.wav) through amp/pedal (reamping), record output of exactly equal length, train. Targets ~3 min reamp audio, ~10 min training.
- A1 WaveNet sizes: standard (~13.8k params), lite, feather, nano; LSTM supported. ESR metric.
- Papers: no single founding paper. Atkinson "Slimmable NAM: Neural Amp Models with adjustable runtime computational cost", arXiv 2511.07470 (Nov 2025, sole author) https://arxiv.org/html/2511.07470v1 . Lineage: Damskägg, Juvela, Välimäki "Real-Time Modeling of Audio Distortion Circuits with Deep Learning" SMC 2019 Best Paper (https://research.aalto.fi/en/publications/real-time-modeling-of-audio-distortion-circuits-with-deep-learnin/); Wright et al. Applied Sciences 2020 (https://www.mdpi.com/2076-3417/10/3/766). Atkinson says he did not work from these → parallel line of work.

## 2. NAM A2 (Architecture 2)
- 17 Jan 2026 Atkinson blog "Architecture 'A2'"; TONE3000 "generously supporting". https://www.neuralampmodeler.com/post/architecture-a2
- 20 Jan 2026 TONE3000 "Announcing A2": "TONE3000 is fully funding the development of A2", target Mar 2026. https://www.tone3000.com/blog/introducing-neural-amp-modeler-nam-architecture-2-a2
- Released 2 Jun 2026 ("A2 is released"): trainer v0.13.0, Core v0.5.2, plugin v0.7.14; train on TONE3000, Colab, or local GUI. v0.13.0 notes: "slimmable WaveNet via channel-slicing", "Packed WaveNet training". https://www.neuralampmodeler.com/post/a2-is-released ; https://github.com/sdatkinson/neural-amp-modeler/releases/tag/v0.13.0
- Rationale: NAM runs on PCs, Raspberry Pi, embedded pedals, websites ("Few technologies in audio are used in more places than NAM"). Goals: 1) keep/improve accuracy; 2) cut CPU ("the most interesting avenue"; "full-size A2 to be comparable to A1, and for slimmed A2 to be an attractive solution for more compute-constrained applications"); 3) keep training ~10 min; 4) keep recording ~3 min. "Nothing is going away" (A1 still supported).
- A2 Full vs Lite (TONE3000 "NAM A2: The Complete Guide" https://www.tone3000.com/guides/nam-a2-the-complete-guide): "An A2 download is a single NAM model that can be run as either A2-Full or A2-Lite" — two "slim points" of one network (8 channels Full, 3 Lite), each with own weight block, trained together (packed training). "TONE3000 creates one A2 model which automatically contains both sizes."
  - A2 Full: studio/desktop; "roughly 30–40% more performant than A1-Standard".
  - A2 Lite: pedals/amps; "runs at 50% CPU on an ARM Cortex M7 600MHz" ("runs on a $3 chip"); ≈ A1-Standard in listening tests. M-series MacBook: ~64 Full or ~200 Lite simultaneously.
  - Tech changes: activation Tanh → LeakyReLU (cheaper; saved CPU spent on bigger net); output stage uses window of samples; receptive field ~6,350 samples (~132 ms @48k) vs ~85 ms; training data normalised to −18 dB RMS; compact binary `.namb` for embedded "with no quality loss"; median ESR on TONE3000 library 0.0062 (A1-Standard) → 0.0033 (A2-Full).
  - Compatibility: "A2 is a new architecture, not a drop-in update" — firmware/software updates needed. TONE3000 retrained library; where originals missing, trained from synthetic data generated by A1 models (labelled "Convert").
  - License: "A2 is fully MIT-licensed" (core, trainer, embedded loader, pure-C engine, WASM engine). Reference pedal: https://github.com/tone-3000/nam-pedal (STM32H750 / Cortex-M7, .namb).
- Blind test: MUSHRA; data CC-BY-4.0 https://github.com/tone-3000/a2-mushra-data : 105,842 ratings, 1,184 participants, 37 tones, 7 systems. Guitar World 2 Jun 2026: A2 Full 100, Neural DSP V2 94, TONEX 91, Line 6 Proxy 77 (appear normalised/relative). https://www.guitarworld.com/gear/amp-modeler-pedals/tone3000-nam-architecture-2 . TONE3000: "The most accurate and best sounding amp modeling technology in history. Open source. Runs on a $3 chip." Vendor claim. The Gear Forum user Sedaxel reanalysis: many raters scored hidden reference <80; after removing unreliable raters A2-Full vs A1 gap (92 vs 88) "not that big"; A2-Lite "more or less on par to the QC V2". https://thegearforum.com/threads/neural-amp-modeler-a2-beats-neural-dsp-v2-tonex-and-line6-in-blind-listening-tests-by-a-lot.11463/
- Other press: https://musictech.com/news/gear/tone3000-nam-architecture-a2/ ; https://sonicstate.com/news/2026/06/12/neural-amp-modeler-a2-released-/ ; https://www.tone3000.com/blog/a2-next-generation-neural-amp-modeler ; https://www.neuralampmodeler.com/post/an-early-glimpse-at-a2

## 3. NAM hardware
"Native" = runs real NAM model; "NAM-to-X" (Atkinson 29 Jul 2025, https://www.neuralampmodeler.com/post/let-s-talk-about-nam-to-x) = converts .nam to proprietary format (approximation).
| Date | Product | Notes |
|---|---|---|
| 8 Mar 2023 | Raspberry Pi / LV2 (Mike Oliphant) | https://blog.nostatic.org/2023/03/neural-amp-modeler-nam-running-on.html |
| 29 Jun 2023 | MOD Dwarf / Duo X | nano only, ~66% CPU https://forum.mod.audio/t/the-neural-amp-modeler-nam-has-arrived/10119 |
| 2023 | Poly Effects Beebo | feather only |
| Oct 2023 ann.; review 27 May 2024 | Dimehead NAM Player (~$500) | "first native NAM pedal"; now A2-Full https://www.tone3000.com/blog/nam-in-a-pedal-it-s-here-it-s-alive-it-s-great |
| 11 Apr 2025 | Valeton GP-200/JR, Hotone Ampero II | NAM "Clone" import (conversion) |
| 22 Apr 2025 | Darkglass Anagram (bass, $1,199.99) | KosmOS 1.16 (27 Jun 2026) up to 3 instances A2 Full/Lite/A1 https://notreble.com/buzz/2026/06/27/darkglass-anagram-gets-nam-a2-full-support-in-kosmos-1-16-update/ |
| 27 May 2025 | Valeton GP-5 | NAM-to-SnapTone |
| 29 Jul 2025 | NUX Amp Academy Stomp | NAM-to-Image |
| 31 Jan 2026 NAMM | Blackstar Beam Mini | native A2, TONE3000 built in; "Rather than build a walled garden… we chose to partner with TONE3000" https://tubesandcode.studio/posts/namm-2026-blackstar-calls-out-tone-walled-gardens-by-unveiling-practice-amp-powered-by-tone3000 |
| 22 Jul 2026 | Hotone Ampero II Stage fw 1.7.0 | native A2 Lite only |
| Jul 2026 | Mooer GS1000/GE1000/GE300 | A2-Lite |
| 1 Aug 2026 | HeadRush Prime/Core/Flex Prime fw 5.1 | native A1+A2, Full/Lite switch, browse TONE3000 on device (dates vary 1/2/23 Aug) https://www.tone3000.com/blog/headrush-nam-tone3000 ; https://www.inmusicbrands.com/press/tone-3000/ |
| other | Sonulab Stompstation Pro, NUX MG (A2-Lite); Lava Music, Chaos Audio, Octave HS-1, Two Notes, Melda, Poly Effects | |
No official NAM certification; closest = "Builders" on neuralampmodeler.com and TONE3000 partner lists.

## 4. TONE3000 (formerly ToneHunt)
- ToneHunt.org early 2023, community project from NAM FB group; open-source repo olilarkin/tonehunt (MIT, Remix + Supabase) https://github.com/olilarkin/tonehunt . Original founders [UNVERIFIED].
- 6 May 2024 "Welcome to the new ToneHunt": rebuilt, code closed, donations. https://www.tone3000.com/blog/welcome-to-the-new-tonehunt
- Jun 2024 absorbed AIDA-X & Proteus models after AIDA-DSP cloud shutdown (https://overdriven.fr/overdriven/index.php/2024/06/23/overdriven-aida-x-models-on-tonehunt-org/); still hosted? [UNVERIFIED]
- TONEZONE3000 (free cloud training) merged; TGF thread starts 8 Nov 2024. https://thegearforum.com/threads/tone3000-tone3000-com-previously-tonehunt-and-tonezone3000.6919/
- 13 Mar 2025 "ToneHunt is now TONE3000" by Woody & Stanley = Woodbury Shortridge (Co-founder/CTO) & Stanley Vergilis (Co-founder/CEO): "our favorite gear has numbers in its name… so why not ours?" https://www.tone3000.com/blog/tonehunt-is-now-tone3000
- Services: NAM + IR library; free cloud Capture (RTX 4090; upload sweep recording or dry/wet pair; "WAV 24-bit, 48kHz, Mono recommended") https://www.tone3000.com/capture ; API 20 Jun 2025 https://www.tone3000.com/blog/introducing-the-tone3000-api ; TONE3000 Plugin 1 Sep 2026 (open source, A2, VST3/AU/AAX/CLAP/LV2/standalone, input Calibration, chains, stereo, EQ) https://www.tone3000.com/blog/tone3000-launches-free-nam-a2-plugin
- Scale (self-reported): Jan 2026 250k captures/4M downloads; Jun 2026 350k+ captures; Sep 2026 700k+ tones, 600k+ MAU, 9M+ downloads.
- Business: free for users; venture-backed (About page) [amount UNVERIFIED]. Vergilis: "Because the TONE3000 Plugin is open source, right now is the worst it will ever be." Shortridge: "NAM isn't a company… it's code that lives on GitHub." https://www.tone3000.com/about
- Why usable: open JSON format, free open-source players, standardised capture signal, community contributions.
- Requirements: NAM-compatible player (Gateway, TONE3000 Plugin, supported hardware; A2 needs updated fw), 48 kHz reference (plugin resamples), input calibration via dBu metadata, separate IR loader for amp-only captures, CPU (A2 Full desktop / A2 Lite pedals; old MOD Dwarf/Beebo only nano/feather).

## 5. Reception & limitations
- From 2023 forums rated NAM most accurate capture; "wiping the floor with capturing a amp… Downside is its glitchy and finicky." Some: "too hifi… more amp than the real amp." TGF Sep 2024: Kemper faster, no computer; NAM captures inconsistent gain → "really difficult to trust" → dBu calibration response. https://thegearforum.com/threads/captures-which-device-platform.6477/ ; https://thegearforum.com/threads/nam-neural-amp-modeler.1698/
- Snapshots (one knob setting). Parametric: Atkinson ParametricOD free plugin (20 Jan 2024) https://www.neuralampmodeler.com/post/the-first-publicly-available-parametric-neural-amp-model ; licensed parametric NAM in SubMission Audio PreFire (May 2025) https://www.neuralampmodeler.com/post/submission-audio-releases-prefire-powered-by-parametric-neural-amp-models , Artera Superior Drive, ML Sound Lab Amped Volcano. Not in open trainer's default pipeline.
- PANAMA (arXiv 2509.26564, ETH): parametric models from as few as 75 knob settings. https://arxiv.org/html/2509.26564v1
- CPU cost main hardware barrier → reason for A2.

## Timeline
2019 start · 2020-02-21 v0.1.0 · 2022-02 plugin · 2022-11 universal loader · early 2023 viral + ToneHunt · 2023-06 MOD Dwarf · 2023-10 Dimehead · 2024-01 ParametricOD · 2024-05 ToneHunt rebuilt · 2025-03-13 TONE3000 · 2025-05 Atkinson full-time · 2025-06 API · 2025-07 NAM-to-X post · 2025-11 Slimmable NAM paper · 2026-01-17/20 A2 announced · 2026-01-31 Blackstar Beam Mini · Mar–Apr 2026 listening tests · 2026-06-02 A2 released · 2026-06-27 Darkglass A2 · 2026-07-22 Hotone A2 Lite · 2026-08-01 HeadRush · 2026-09-01 TONE3000 Plugin

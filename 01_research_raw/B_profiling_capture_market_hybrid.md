# Raw research B — PROFILING / CAPTURE, 2026 market, HYBRID approaches
Research date: 2026-09-27. Source: research sub-agent report (English). [UNVERIFIED] = no confirming primary source.

## 1. Kemper Profiler
- Christoph Kemper (Access Virus synth) moved into guitar products 2006; Profiler announced NAMM 2011 (head/rack). SoS review Apr 2012. https://en.wikipedia.org/wiki/Kemper_Profiler ; https://www.soundonsound.com/reviews/kemper-profiling-amplifier
- Profiler Player revealed Dec 2023 (€698; initially play-only). https://www.soundonsound.com/news/kemper-reveal-profiler-player
- Profiler MK 2 series announced 29 May 2025 (Head $1,348, Stage $1,498, Player $699; 20 FX blocks; 8-ch USB audio). https://www.soundonsound.com/news/kemper-unveil-profiler-mk-2-series ; https://www.kemper-amps.com/news/95/Introducing-the-all-new-KEMPER-PROFILER-MK-2
- Liquid Profiling: announced ~May–Jul 2023; public beta OS 10, 3 Aug 2023, free. https://www.guitarworld.com/news/kemper-liquid-profiling-launch ; https://www.guitarworld.com/news/kemper-liquid-profile-update
- Profiling 2.0: 5 Mar 2026, public beta OS 14.0 for MK 2 & Player: ">100,000 individual frequency points", "authentic gain detection", cab resonance capture, "Smart DI profiling", cab morphing; Player can now create profiles; non-MK2 play 2.0 profiles at lower resolution. https://www.premierguitar.com/news/kemper-profiling-technology-20 ; https://www.kemper-amps.com/news
- How it works: test signals ("white noise at different frequencies", Wikipedia) into real amp, mic/DI back; forum expert: signal "starts quiet and gets really loud" → estimates gain-dependent distortion (Kemper forum 64273). NOT pure black box: fits onto its own adjustable internal amp structure; after profiling users can still tweak compression, sag, pick attack etc. Forum: Kemper "is a modeler, it just automates the code when you profile" (thread 54173). Cliff Chase 2015 claimed Kemper uses "one of seven amp models" plus EQ; Kemper staff rejected it (thread 23686). Internals never published [UNVERIFIED].
- Profile types: Studio (amp+cab+mic; "Cab Driver" separates approx.), Direct (DI, no cab), Merged (direct amp + studio cab). https://overdriven.fr/overdriven/index.php/2024/01/23/kemper-profiles-for-beginners-part-1/
- Patent: US 8,796,530 B2 "Musical instrument with acoustic transducer", inventor Christoph Kemper; priority 29 Jul 2006 (DE 10 2006 035 188 B4; EP 1 883 064 B1; US 2008/0134867 A1); granted 5 Aug 2014; expiry 27 Jul 2027 per Google Patents. Describes reference profile via mic + closed loop minimizing difference between model profile and reference = profiling. https://patents.google.com/patent/US8796530B2/en . Fractal forum claims a Kemper profiling patent expired 15 Jun 2022 (https://forum.fractalaudio.com/threads/june-15-2022-the-day-the-kemper-profiler-patent-expires.177111/) — which patent/date [UNVERIFIED]. Liquid Profiling "patented" no number found.
- Limitations: snapshot of one knob setting (SoS 2012: "the amp's controls at specific settings, and the amp miked up in a certain way"); generic EQ before Liquid.
- Blind test: Düvel et al. 2020, 177 listeners: 56.2% correct, d′=0.34 (barely above chance). https://journals.sagepub.com/doi/full/10.1177/2059204320901952
- Liquid Profiling = hybrid: modeled gain controls, tone stack, presence for ~40 real amps at launch (Fender Super/Deluxe Reverb, Marshall JTM45/Plexi, Vox AC30…); pick matching "amp model" and set knobs as when profiled. Christoph Kemper: "the perfect marriage of modeling and profiling or profiling modeling"; "This is a profile, not a modeling amp. We didn't want to model the distortion or the cabinet – just what you have in your hands." Tone stack placement before/after gain stages matters (forum 64273). Power amp/speaker/cab remain captured.

## 2. Neural DSP
- Founded 2017. Quad Cortex announced NAMM Jan 2020 w/ Neural Capture; shipped ~late 2020/2021 [exact UNVERIFIED]. https://en.wikipedia.org/wiki/Neural_DSP
- Nano Cortex announced 18 Sep 2024, $549, creates captures on device. https://neuraldsp.com/nano-cortex-updates/neural-dsp-technologies-introduces-nano-cortex
- TINA (Telemetric Inductive Nodal Actuator) introduced 31 Jul 2024 w/ CorOS 3.0: robot turns every knob, "typically thousands of control positions"; NN learns amp at any setting. First on QC: Archetype Plini X and Gojira X. https://neuraldsp.com/quad-cortex-updates/introducing-tina ; https://neuraldsp.com/news/neural-dsp-amplifier-modeling-technology ("a 6-knob amp with 10 settings each = 1 million combinations")
- Neural Capture V2: 26 Nov 2025; training in Cortex Cloud (~10 min, up to 40/day); better on fuzz, compressors, sag; created on QC (CorOS 3.3.0); Nano can play not create. https://neuraldsp.com/news/introducing-neural-capture-version-2
- Quad Cortex mini: NAMM 23 Jan 2026, €1,299. https://www.soundonsound.com/news/namm-2026-neural-dsp-introduce-quad-cortex-mini
- Research lineage: Wright, Damskägg, Juvela, Välimäki 2020 (Damskägg & Juvela at Neural DSP): "three minutes of audio data is sufficient". https://www.mdpi.com/2076-3417/10/3/766

## 3. Other capture / matching
- IK TONEX ("AI Machine Modeling"): software late 2022 [month UNVERIFIED]; GW review 5 Jan 2023; ~5 min test signal; "models a tonal snapshot rather than the entire amp… at component level"; TONEX Pedal Feb 2023 €399.99; ToneNET 65,000+. https://www.ikmultimedia.com/products/tonex/ ; https://www.guitarworld.com/reviews/ik-multimedia-tonex-review ; https://www.musicradar.com/news/ik-multimedia-tonex-pedal
- Line 6 Helix Stadium: announced 13 Jun 2025 (Agoura modeling, "Hype" control), XL $2,199.99 / Floor $1,799.99 (Floor shipped mid-Feb 2026). Proxy (fw 1.3, ~24 Mar 2026): test signals → Line 6 cloud → "Clone"; types Amp+Cab / Amp / Preamp / Distortion; mono; max 4 per preset; "Phase 1". https://guitar.com/news/gear-news/line-6-helix-stadium/ ; https://www.sweetwater.com/sweetcare/articles/line-6-helix-stadium-proxy-cloning-engine-guide/ ; https://namforum.com/wiki/index.php/Line_6_Proxy ; https://ilikekillnerds.com/2026/03/07/helix-stadium-proxy-might-be-smarter-than-the-capture-arms-race/
- Fractal Tone Match: reference vs local signal, spectral compare, correction filter after modeled amp/cab; start "reasonably close… same or similar amp type". Axe-Fx II era ~2011–12 [date UNVERIFIED]. https://www.fractalaudio.com/downloads/manuals/axe-fx-2/Axe-Fx-II-Tone-Match-Manual.pdf
- Positive Grid BIAS Amp Match (BIAS Professional, SoS Jan 2015): records source & target, "applies some processing to 'match' the two"; needs close starting model. https://www.soundonsound.com/reviews/positive-grid-bias
- Positive Grid BIAS X (23 Sep 2025, $149): 33 amps, 62 FX; Text-to-Tone; Music-to-Tone (rebuild tone from a song, "trained on over a million tones"). https://www.guitarworld.com/gear/plugins-apps/positive-grid-bias-x-launch
- Positive Grid Spark PEDAL (9 Sep 2026, $199): HD Tone Engine, 33 amps, ToneCloud 100k+, Text-to-Tone; no capture. https://www.premierguitar.com/news/positive-grid-announces-spark-pedal
- Hotone Sound Clone (17 Mar 2025), Ampero II; converts NAM to .clo; later NAM A2-Lite. hotone.com
- Two Notes Genome 2.0 (18 Jun 2026, $129.99): PARADEX "multi-parametric AmpNet capture"; CODEX loads NAM/AIDA-X/Proteus; TSM-Ai amps "AI capture algorithms with precisely modeled tone stacks and power amp emulations" (explicit hybrid); DynIR. https://rekkerd.org/two-notes-releases-genome-2-0-all-in-one-guitar-bass-amplifier-modeling-suite/ ; https://www.two-notes.com/en/discover-genome-2/ ; https://guitarbomb.com/blog/two-notes-genome-2-0/
- Mooer GS1000 "AI Intelligent Amp Sampling Processor", .GNR/.GIR [tech UNVERIFIED]. https://www.mooeraudio.com/pro/32.html

## 4. Market table (Sep 2026)
| Brand | Products | Core tech | Year | Notes |
|---|---|---|---|---|
| Line 6 (Yamaha) | Helix / HX Stomp / POD Go | Modeling (HX) + IR | 2015 / 2019 / 2020 [UNVERIFIED years] | |
| Line 6 | Helix Stadium / XL | Agoura modeling + Proxy capture | 2025; Proxy 2026 | clones cloud-trained, mono |
| Fractal | Axe-Fx III, FM9, FM3, VP4, AM4 | Component modeling + IR; Tone Match | 2006…2025 | no neural capture |
| Neural DSP | Quad Cortex / QC mini / Nano Cortex / Archetype plugins | Neural capture + TINA knob-conditioned neural models + IR | 2020 / 2026 / 2024 | Capture V2 Nov 2025 |
| Kemper | Profiler Head/Rack/Stage/Player, MK 2 | Profiling (parametric fit) + Liquid hybrid | 2011; 2023; 2025; P2.0 2026 | |
| IK Multimedia | TONEX sw/Pedal/ONE/Plug; AmpliTube | Neural capture + IR; AmpliTube = modeling | 2022/2023 | ToneNET 65k+ |
| Fender | Tone Master Pro | Modeling + IR | 2023 | Jun 2026 fw +8 amps (https://www.musicradar.com/guitars/fender-tone-master-pro-firmware-update-2026) |
| Boss | GT-1000 / Katana Gen 3 | Modeling (AIRD / Tube Logic) | 2018 / May 2024 | https://www.boss.info/us/whats_new/press_releases/2024/BOSS-Introduces-Katana-Gen-3-Guitar-Amplifier-Seri/ |
| Positive Grid | BIAS Amp (Amp Match), BIAS FX, BIAS X, Spark 2, Spark PEDAL | Modeling; Amp Match = model+EQ match; BIAS X = AI tone matching | 2014–15; 2025; 2026 | no hardware capture |
| Universal Audio | UAFX Dream '65 / Ruby '63 / Woodrow '55 / Lion '68 | Circuit modeling + OX speaker/mic/room | May 2022 | https://www.soundonsound.com/news/universal-audio-announce-uafx-amp-emulators |
| Strymon | Iridium | Modeling + IR | 2020 [UNVERIFIED] | https://www.strymon.net/product/iridium/ |
| HeadRush | Prime / Core / Flex Prime | Modeling + NAM A2 | fw 5.1 Jul/Aug 2026 | TONE3000 browse on device |
| Hotone | Ampero II | Modeling + Sound Clone + NAM | 2025–26 | .clo |
| Mooer | GS1000 / GE1000 / GE300 | Modeling + AI sampling (GNR) + NAM A2-Lite | 2024–26 | |
| Two Notes | Torpedo Captor X / OPUS / ReVolt; Genome 2.0 | DynIR; TSM-Ai hybrid; PARADEX; NAM loading | 2026 | |
| Dimehead / Darkglass / Sonulab | NAM Player / Anagram / Stompstation Pro | NAM playback (A2-Full) | 2024–26 | https://www.gearnews.com/neural-amp-modeler-guide-guitar/ |

## 5. Hybrid: model as base + capture data to correct
Commercial, least→most sophisticated:
1. Static EQ correction after modeled amp: Fractal Tone Match (~2012), BIAS Amp Match (~2014–15). Correct only at matched setting.
2. Profile + modeled tone stack: Kemper Liquid Profiling (2023) — reversed direction (capture base, modeled controls).
3. Capture-based amps w/ modeled parts: Two Notes TSM-Ai; Genome 2.0 PARADEX (2026).
4. Knob-conditioned neural models trained on robot data: Neural DSP TINA (2024).
5. Two tracks in one ecosystem: Line 6 Stadium (Agoura + Proxy); HeadRush/Hotone (modeling + NAM).
6. AI matching model parameters to a reference recording: Positive Grid BIAS X Music-to-Tone (2025) — output is settings on a model, so knobs keep working.
Academic grey-box:
| Paper | Idea | Result |
|---|---|---|
| Kuznetsov, Parker, Esqueda (NI), Differentiable IIR Filters for ML Applications, DAFx 2020 https://www.dafx.de/paper-archive/2020/proceedings/papers/DAFx2020_paper_52.pdf | IIR = linear RNN; Wiener-Hammerstein w/ MLP | Boss DS-1 comparable to measurement approaches |
| Esqueda, Kuznetsov, Parker, Differentiable White-Box Virtual Analog Modeling, DAFx20in21 https://dafx.de/paper-archive/2021/proceedings/papers/DAFx20in21_paper_39.pdf | learn component values from audio | RC tolerances, FMV tone-stack pot curves, TS-808 diode asymmetry (MSE 6.4e-3→1.5e-3) |
| Miklánek, Wright, Välimäki, Schimmel, Neural Grey-Box Guitar Amplifier Modelling with Limited Data, DAFx 2023 https://www.dafx.de/paper-archive/2023/DAFx23_paper_52.pdf | LSTM preamp → white-box differentiable tone stack → GRU power amp (Marshall JVM 410H) | 4 min at one setting generalises to unseen tone-stack settings; MUSHRA 87–95 vs 17–47 for black-box RNNs (trained on 84 min/21 settings) |
| Comunità, Steinmetz, Reiss, Differentiable black-box and gray-box modeling of nonlinear audio effects, Frontiers Signal Proc. Jul 2025 https://www.frontiersin.org/journals/signal-processing/articles/10.3389/frsip.2025.1580395/full | large comparison | grey-box less data/params, interpretable, but "cannot achieve state-of-the-art emulation accuracy" |
Feasibility synthesis: feasible and shipping. Attractive because tone stacks are linear & cheap to model exactly, nonlinear feel best captured; DAFx 2023 ~20x less data. Costs: need prior topology knowledge (lose "capture any amp blindly"); feedback/PI interaction hard; grey-box trails best black-box on raw accuracy; TINA brute force only for manufacturers.

## 6. Community quotes
- Neural DSP forum mod MP_Mod (Apr 2023): "Captures are… a snapshot of settings in an exact moment in time. Models are created to give full control to gain, EQ and related." https://unity.neuraldsp.com/t/modeling-vs-captures-i-am-not-clear-on-this/10743
- Kemper forum 2021: "Profiler – Xerox copy machine"; profilers "need massively less processing power"; can "profile (almost) any amp… without prior knowledge". https://forum.kemper-amps.com/forum/thread/54173-understanding-the-differences-between-modelers-and-profilers/
- The Gear Forum Jul 2023 on Liquid: "genuine breakthrough-game-changer" vs "Component-level modelling already does this". https://thegearforum.com/threads/kemper-liquid-profiling-first-look.2733/
- Kemper forum Aug 2023: Liquid stacks "certainly more accurate than the generic Kemper tone". https://forum.kemper-amps.com/forum/thread/61353-liquid-profile-tone-stack-accuracy/
- ilikekillnerds Mar 2026: "real guitarists want both".
Blocked: TGP, Reddit.

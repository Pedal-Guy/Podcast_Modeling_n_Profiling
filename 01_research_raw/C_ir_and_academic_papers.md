# Raw research C — IR/convolution, system identification, circuit & neural modeling (academic)
Research date: 2026-09-27. Source: research sub-agent report (English).

## Concepts
- Impulse response (IR): record the output of a system fed an infinitely short click. For an LTI (linear, time-invariant) system, the IR fully describes it. Output = input convolved with IR (each input sample triggers a scaled, delayed copy of the IR; all summed) — this is what an IR loader does.
- Measurement: Angelo Farina's exponential sine sweep (AES 108th, Paris, Feb 2000, paper 5093; refined AES 122nd 2007). Targets "weakly nonlinear, approximately time-invariant systems"; harmonic distortion products appear as separate "pre-echoes" before the linear IR. https://www.aes.org/e-lib/browse.cfm?elib=10211
- Cab + mic ≈ LTI at normal levels → one IR captures frequency response, phase, decay. Limit (Guitar World): "Impulse responses are a really accurate way of capturing the sound of a guitar cabinet, but they're linear"; cone breakup & voice-coil power compression are level-dependent; "the IR is taken at one level." https://www.guitarworld.com/gear/plugins-apps/what-are-impulse-responses
- Two Notes DynIR = large grid of static IRs (8 mics × ~10k positions ≈ 160k IRs per cab), not a nonlinearity fix. https://www.two-notes.com/en/dyn-ir/
- Tube amp NOT LTI: nonlinear (clipping, harmonics, touch-dependent) and history-dependent (power-supply sag, bias shift from grid-current charging coupling caps) = "nonlinear with long memory". Novak et al. 2010: polynomial Hammerstein model of Tube Screamer accurate only at measured input level. https://link.springer.com/article/10.1155/2010/793816
- IR is its own technique: a linear building block used by both camps (modellers end chain with cab IR; profilers measure cab as separate linear filter; block-oriented models are literally IR–nonlinearity–IR).
- Commercial convolution: Sony DRE-S777 (SoS Dec 1999: "the first commercial, real-time convolutional reverberator"); Altiverb (SoS review May 2002; release 2001 UNVERIFIED; Sonic Foundry Acoustic Mirror existed earlier); Two Notes Torpedo VB-101 (SoS Jun 2010; dummy load + convolution, 8 virtual mics, ~30 cabs; 2009 ann. UNVERIFIED); Celestion own IRs announced 18 Jan 2017 (https://celestion.com/our-news/celestion-introduces-their-revolutionary-line-of-impulse-responses-irs/); Red Wirez/OwnHammer/York Audio dates UNVERIFIED.
- White-box: schematic → circuit equations (MNA, DK-method, state space, WDF). Grey-box: structural knowledge; block-oriented: Hammerstein (N–L), Wiener (L–N), Wiener-Hammerstein (L–N–L; "the fundamental paradigm of electric guitar tone"). Black-box: I/O only: Volterra series ("work well for relatively linear systems but struggle… very strong nonlinear behaviour"), neural networks. (Vanhatalo 2022 taxonomy)
- Kemper link: Vanhatalo review & Eichas et al. DAFx-17 cite a 2008 Kemper patent describing a Wiener-Hammerstein grey-box approach (second-hand). Later Kemper patents via Justia: US 11,463,057 (2022; two linear transfer functions + "a trivial nonlinearity"), US 12,289,586 (2025). https://patents.justia.com/inventor/christoph-kemper . Vanhatalo says Fractal's MIMIC white paper extends this idea.
- ESR (error-to-signal ratio) = Σ(y−ŷ)²/Σy²; 0 perfect, 0.01 = 1%. Usually after pre-emphasis high-pass (1−0.85z⁻¹ or 1−0.95z⁻¹). Wright & Välimäki ICASSP 2020: A-weighting pre-emphasis best perceived; raw ESR ≠ what ears hear.

## Key papers
| Year | Authors | Title | Venue | Contribution | URL |
|---|---|---|---|---|---|
| 1986 | A. Fettweis | Wave Digital Filters: Theory and Practice | Proc. IEEE 74(2) | WDF foundation | https://ccrma.stanford.edu/~jingjiez/portfolio/gtr-amp-sim/pdfs/Wave%20Digital%20Filters%20Theory%20and%20Practice.pdf |
| 1996 | N. Koren | Improved vacuum tube models for SPICE simulations | Glass Audio 8(5) | standard phenomenological triode model | https://www.normankoren.com/Audio/Tubemodspice_article.html |
| 2000 | A. Farina | Simultaneous measurement of IR and distortion with a swept-sine technique | AES 108th, #5093 | exp. sine sweep | https://www.aes.org/e-lib/browse.cfm?elib=10211 |
| 2006 | M. Karjalainen, J. Pakarinen | Wave digital simulation of a vacuum-tube amplifier | ICASSP 2006 | early WDF tube stage | https://research.aalto.fi/en/publications/wave-digital-simulation-of-a-vacuum-tube-amplifier/ |
| 2009 | D. T. Yeh | Digital Implementation of Musical Distortion Circuits by Analysis and Simulation | PhD Stanford CCRMA | DK-method, Tube Screamer, tone stacks, 12AX7 | https://ccrma.stanford.edu/~dtyeh/papers/DavidYehThesissinglesided.pdf |
| 2009 | J. Pakarinen, D. T. Yeh | A review of digital techniques for modeling vacuum-tube guitar amplifiers | Computer Music Journal 33(2):85–100 | key pre-neural survey | https://direct.mit.edu/comj/article/33/2/85/94251 |
| 2010 | Yeh, Abel, Smith | Automated physical modeling of nonlinear audio circuits for real-time audio effects, Part I | IEEE TASLP 18(4) | DK-method automatic state-space | https://ccrma.stanford.edu/~dtyeh/papers/yeh10_taslp.pdf |
| 2010 | Novák, Simon, Kadlec, Lotton | Nonlinear system identification using exponential swept-sine signal | IEEE TIM 59(8) | generalized Hammerstein from one sweep | https://ieeexplore.ieee.org/document/5299278/ |
| 2010 | Novák, Simon, Lotton | Analysis, synthesis and classification of nonlinear systems using synchronized swept-sine | EURASIP JASP | level dependence breaks single Hammerstein | https://link.springer.com/article/10.1155/2010/793816 |
| 2011 | Macák, Schimmel | Real-time guitar preamp simulation using modified blockwise method | EURASIP JASP | lookup tables for real-time circuits | https://link.springer.com/article/10.1155/2011/629309 |
| 2013 | Covert, Livingston | A vacuum-tube guitar amplifier model using a recurrent neural network | IEEE SoutheastCon | first NN amp model (NARX), "poorer than expected" | https://ieeexplore.ieee.org/document/6567472 |
| 2015 | Werner, Nangia, Smith, Abel | Resolving WDFs with Multiple/Multiport Nonlinearities | DAFx-15 | WDF for real circuits | https://www.ntnu.edu/dafx15/proceedings |
| 2016 | Eichas, Zölzer | Black-box modeling of distortion circuits with block-oriented models | DAFx-16 | extended Wiener, LM fit | https://www.academia.edu/28440297/ |
| 2017 | Schmitz, Embrechts | Hammerstein kernels identification by sine sweep… | J. AES 65(9) | | https://orbi.uliege.be/handle/2268/216243 |
| 2017 | Eichas, Möller, Zölzer | Block-oriented gray box modeling of guitar amplifiers | DAFx-17 | L–N–L–N–L + bias shift; clean ≈ real, high gain median ~50 | https://dafx17.eca.ed.ac.uk/papers/DAFx17_paper_35.pdf |
| 2018 | Eichas, Zölzer | Virtual analog modeling of guitar amplifiers with Wiener-Hammerstein models | (venue UNVERIFIED) | 4 amps, 19 listeners, "minor differences" | https://www.hsu-hh.de/ant/wp-content/uploads/sites/699/2018/04/Eichas_VA_Modeling_of_guitar_amps_with_WH_models.pdf |
| 2018 | Zhang et al. | A vacuum-tube guitar amplifier model using LSTM networks | IEEE SoutheastCon | Vox AC4TV | https://ieeexplore.ieee.org/document/8479039/ |
| 2018 | Schmitz, Embrechts | Nonlinear real-time emulation of a tube amplifier with a LSTM neural-network | AES 144th #9966 | ~2% RMS error | https://aes.org/e-lib/browse.cfm?elib=19483 ; https://arxiv.org/abs/1804.07145 |
| 2019 | Damskägg, Juvela, Thuillier, Välimäki | Deep learning for tube amplifier emulation | ICASSP 2019 | feedforward WaveNet; SPICE Fender Bassman 56F-A; knob-conditioned | https://arxiv.org/abs/1811.00334 |
| 2019 | Wright, Damskägg, Välimäki | Real-time black-box modelling with recurrent neural networks | DAFx-19 | LSTM ≈ WaveNet at fraction of CPU | https://www.dafx.de/paper-archive/2019/DAFx2019_paper_43.pdf |
| 2019 | Schmitz, Embrechts | Objective and subjective comparison of several ML techniques… | AES 146th | listening test | https://orbi.uliege.be/handle/2268/236627 |
| 2020 | Wright, Damskägg, Juvela, Välimäki | Real-time guitar amplifier emulation with deep learning | Applied Sciences 10(3):766 | WaveNet vs LSTM; MUSHRA > 90 | https://www.mdpi.com/2076-3417/10/3/766 |
| 2020 | Wright, Välimäki | Perceptual loss function for neural modelling of audio systems | ICASSP 2020 | A-weighted ESR; MUSHRA 77→86 | https://arxiv.org/abs/1911.08922 |
| 2020 | Düvel, Kopiez, Wolf, Weihe | Confusingly similar: discerning hardware amp sounds and Kemper simulations | Music & Science 3 | 177 listeners, 56.2% | https://journals.sagepub.com/doi/full/10.1177/2059204320901952 |
| 2022 | Vanhatalo et al. | A review of neural network-based emulation of guitar amplifiers | Applied Sciences 12(12):5894 | key neural survey, taxonomy | https://www.mdpi.com/2076-3417/12/12/5894 |
| 2022 | Steinmetz, Reiss | Efficient neural networks for real-time modeling of analog dynamic range compression | AES 152nd / arXiv 2021 | TCN, LA-2A | https://arxiv.org/abs/2102.06200 |
| 2023 | Comunità, Steinmetz, Phan, Reiss | Modelling black-box audio effects with time-varying feature modulation | ICASSP 2023 | TFiLM for long memory | https://arxiv.org/abs/2211.00497 |
| 2023 | Wright, Välimäki, Juvela | Adversarial guitar amplifier modelling with unpaired data | ICASSP 2023 | GAN, no paired data | https://arxiv.org/abs/2211.00943 |
| 2023 | Miklánek, Wright, Välimäki, Schimmel | Neural grey-box guitar amplifier modelling with limited data | DAFx-23 | RNN + nodal-analysis tone stack | https://www.dafx.de/paper-archive/details/UYVDlRJ95vRRQbsqJVUvqw |
Open source: Keith Bloemer GuitarML (SmartGuitarAmp WaveNet; Proteus LSTM on RTNeural; NeuralPi) https://github.com/GuitarML ; https://keyth72.medium.com/guitarml-faq
Corrections: Zhang 2018 is LSTM at SoutheastCon; Schmitz 2018 LSTM at AES 144 not DAFx; Celestion IRs 2017 not 2016.

## Perceptual evidence
- Düvel 2020 (Kemper): 177 listeners (89% guitarists), 1968 Marshall JMP & Vox AC30 vs profiles, A/not-A online; 56.2% correct, d′=0.343; expertise didn't help. "Listeners are rarely able to assign audio examples to the correct condition."
- Wright 2020: Mesa 5:50+ all models >90 MUSHRA (WaveNet3 & RNN-96 scored 100 in ~80% of trials); Blackstar HT-5M ~90; 9 valid listeners. Press (Feb 2020): "first time… blind-test listeners couldn't tell the difference" https://www.sciencedaily.com/releases/2020/02/200211103732.htm ; https://newatlas.com/music/aalto-neural-networks-digital-amp-modeling/
- Eichas grey-box: near perfect clean, weaker heavy distortion.
- No peer-reviewed head-to-head of Kemper vs white-box vs neural. Vendor test: TONE3000 NAM A2 MUSHRA Jun 2026 (vs Neural DSP V2, TONEX V2, Line 6 Proxy; no Kemper), not peer reviewed; forum reanalysis found smaller margins.

## Plain-language framing
- IR = photograph of an echo; cab behaves about the same soft or hard (linear).
- Amp = living thing; play harder → more harmonics, compression, sag. One photo can't capture it. IR is a Lego brick, not the house.
- Three ways to copy an amp: white-box (rebuild circuit in maths), grey-box (filter→distortion→filter), black-box (let a neural net listen). Neural went from "poor" (2013) to "listeners couldn't tell" (2020).

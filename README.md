# Awesome-GenAI-Watermarking

A curated reading list of watermarking for **generative AI across images, video, speech and audio, text, code, and 3D content**. It covers watermarks embedded during training, during generation, and after generation, together with attacks, evaluation, theory, and software.

**Last literature and metadata review: 19 October 2026.** This is a broad, curated bibliography, not an exhaustive inventory. Foundational multimedia methods are included when they are useful baselines for generative-content watermarking. Training-data attribution and model ownership verification have their own section because their goals differ from identifying a generated asset.

## Contents

- [Reading guide and terminology](#reading-guide)
- [Images](#images)
  - [Model training and adaptation](#image-model)
  - [Diffusion sampling and latent-noise methods](#image-inference)
  - [Autoregressive image generation](#image-ar)
  - [Post-generation image watermarks and foundations](#image-post)
- [Video](#video)
- [Speech, audio, and music](#audio)
  - [Waveform and codec-based embedding](#audio-post)
  - [Speech and audio model integration](#audio-model)
  - [Autoregressive audio sampling](#audio-inference)
- [Text and language models](#text)
  - [Token sampling and adaptive watermarking](#text-token)
  - [Semantic, sentence, and prompt-based methods](#text-semantic)
  - [Post-generation text watermarking](#text-post)
  - [Multi-bit messages and attribution](#text-multibit)
  - [Public verification and spoofing resistance](#text-public)
  - [Watermarks learned or embedded in model weights](#text-model)
  - [Diffusion language models](#text-diffusion)
- [Source code](#code)
- [3D scenes and neural rendering](#3d)
- [Training-data attribution and model ownership](#ownership)
- [Attacks and robustness analysis](#attacks)
- [Benchmarks, detection, and evaluation](#bench)
- [Theory and security foundations](#theory)
- [Surveys](#survey)
- [Software and provenance standards](#software)
- [Related work and archival entries](#related)
- [Maintenance and contribution notes](#maintenance)

<a id="reading-guide"></a>
## Reading guide and terminology

**Watermarking deliberately inserts a signal into content or a model.** Passive AI-content detectors instead look for traces that arise without deliberate embedding. A watermark may indicate presence (zero-bit detection), carry an identifier or message (multi-bit decoding), support source attribution, or help localize tampering. Some ownership schemes respond only to special trigger inputs; they do not watermark every ordinary output.

| Insertion point | What changes | Representative examples |
| --- | --- | --- |
| Training data | Mark examples before training and test whether the signal is learned | Artificial Fingerprinting, DiffusionShield, radioactive watermarks |
| Model training or adaptation | Modify generator weights, adapters, or a decoder so outputs carry a signal | Stable Signature, AquaLoRA, GaussMark |
| Generation procedure | Modify sampling, logits, latent noise, guidance, or prompt instructions | KGW, SynthID-Text, Tree-Ring, Gaussian Shading, In-Context Watermarks |
| After generation | Transform a completed image, waveform, text, or program | TrustMark, AudioSeal, PostMark, SrcMarker |

These categories can overlap. A method may train an auxiliary encoder while leaving the generator unchanged, or combine model adaptation with generation-time controls. "Training-free" should therefore be read with respect to the specific components in the paper. Embedding a watermark in model weights does not by itself guarantee that it survives fine-tuning, extraction, distillation, or other modifications.

For comparisons, check the **threat model and operating point**, not just the reported accuracy:

- **Quality and utility:** perceptual quality, text meaning, executable code behavior, and generation latency.
- **Detection and attribution:** false-positive rate, detection power, sample length, payload capacity, and number of candidate keys or users. AUC and raw bit accuracy alone do not establish deployment reliability.
- **Robustness:** ordinary edits, compression, paraphrasing, translation, regeneration, cropping, resynthesis, and model modifications.
- **Security:** removal, forgery/spoofing, key inference, collusion, and overwriting; distinguish secret-key access, detector queries, generator queries, and access to model weights.
- **Guarantees:** statistical, computational, and empirical claims have different assumptions. "Unbiased," "distortion-free," "undetectable," and "unforgeable" are not interchangeable.

A positive watermark test is evidence relative to a particular scheme, key, threshold, and threat model. It is not a general proof that the content is truthful or that a claimant owns its copyright. Absence of a watermark is not evidence that content is human-created.

### How to read the tables

**Year** is the publication year where verified; for an unconfirmed publication it is the first preprint year. **Preprint** means that a later peer-reviewed venue was not verified in this review, not that one cannot exist. Workshop and Findings papers are labeled separately from main-conference papers. Acceptance reported only by authors is explicitly identified.

Paper titles link to proceedings, publisher pages, or author manuscripts. **Code** links identify author-linked implementations; **Project**, **Artifact**, **Audio samples**, **Reading list**, and **Repo placeholder** are labeled separately. **—** means no official code or project resource was verified. Repository availability is not a claim that the code was executed or remains maintained. Rows are sorted newest first within each subsection; each paper appears once.


## Images

Output watermarking for image generators and reusable image-watermarking baselines.


<a id="image-model"></a>
### Model training and adaptation

Methods that adapt a generator, decoder, or model-associated module. Trigger-only ownership schemes are listed under [model ownership](#model-ip).

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [Learning to Watermark in the Latent Space of Generative Models](https://proceedings.mlr.press/v306/rebuffi26a.html) | ICML | [Code](https://github.com/facebookresearch/distseal) | DistSeal: learned watermarking in generative-model latent spaces. |
| 2026 | [MOLM: Mixture of LoRA Markers](https://proceedings.iclr.cc/paper_files/paper/2026/hash/c4a581f8219f62e3e5c83d4d80be5c9d-Abstract-Conference.html) | ICLR | [Code](https://github.com/Samar-Fares/MOLM-Watermark) | Key-dependent routing through LoRA adapters for configurable image watermarks. |
| 2025 | [Latent Watermark: Inject and Detect Watermarks in Latent Diffusion Space](https://arxiv.org/abs/2404.00230) | IEEE TMM | [Code](https://github.com/RichardSunnyMeng/LatentWatermark-official-codes) | Injects and detects watermarks in latent diffusion space using progressive training. |
| 2025 | [Secure and Efficient Watermarking for Latent Diffusion Models in Model Distribution Scenarios](https://www.ijcai.org/proceedings/2025/831) | IJCAI | [Code](https://github.com/REPO-EXP/DistriMark) | DistriMark: combines latent watermark embedding with model-associated verification constraints. |
| 2025 | [WMAdapter: Adding WaterMark Control to Latent Diffusion Models](https://proceedings.mlr.press/v267/ci25a.html) | ICML | [Code](https://github.com/showlab/WMAdapter) | Contextual adapters add configurable messages to latent diffusion models. |
| 2024 | [AquaLoRA: Toward White-box Protection for Customized Stable Diffusion Models via Watermark LoRA](https://proceedings.mlr.press/v235/feng24k.html) | ICML | [Code](https://github.com/Georgefwt/AquaLoRA) | LoRA-based watermark embedding for customized diffusion models. |
| 2024 | [LaWa: Using Latent Space for In-Generation Image Watermarking](https://arxiv.org/abs/2408.05868) | ECCV | [Code](https://github.com/vbdi/LaWa) | Latent-space embedding during image generation. |
| 2024 | [Safe-SD: Safe and Traceable Stable Diffusion with Text Prompt Trigger for Invisible Generative Watermarking](https://doi.org/10.1145/3664647.3681418) | ACM MM | [Code](https://github.com/AnonymousPony/safe-sd) | Prompt-triggered generative watermarking with a fine-tuned latent diffuser. |
| 2024 | [WOUAF: Weight Modulation for User Attribution and Fingerprinting in Text-to-Image Diffusion Models](https://openaccess.thecvf.com/content/CVPR2024/html/Kim_WOUAF_Weight_Modulation_for_User_Attribution_and_Fingerprinting_in_Text-to-Image_CVPR_2024_paper.html) · [arXiv](https://arxiv.org/abs/2306.04744) | CVPR | [Code](https://github.com/kylemin/WOUAF) | Weight modulation for user-specific fingerprints in text-to-image diffusion models. |
| 2023 | [Catch You Everything Everywhere: Guarding Textual Inversion via Concept Watermarking](https://arxiv.org/abs/2309.05940) | Preprint | — | Embeds user-identifying watermarks into textual-inversion concepts. |
| 2023 | [Flexible and Secure Watermarking for Latent Diffusion Model](https://dl.acm.org/doi/10.1145/3581783.3612448) | ACM MM | — | Message-conditioned latent diffusion watermarking without separate fine-tuning for each payload. |
| 2023 | [PTW: Pivotal Tuning Watermarking for Pre-Trained Image Generators](https://www.usenix.org/conference/usenixsecurity23/presentation/lukas) · [arXiv](https://arxiv.org/abs/2304.07361) | USENIX Security | [Code](https://github.com/nilslukas/gan-watermark) | Pivotal tuning of pretrained image generators for fingerprinted outputs. |
| 2023 | [Securing Deep Generative Models with Universal Adversarial Signature](https://arxiv.org/abs/2305.16310) | Preprint | [Code](https://github.com/zengxianyu/genwm) | Fine-tunes generators on images carrying a learned adversarial signature. |
| 2023 | [Stable Messenger: Steganography for Message-Concealed Image Generation](https://arxiv.org/abs/2312.01284) | Preprint | — | Latent-aware message-concealed image generation; also studies whole-message recovery losses. |
| 2023 | [The Stable Signature: Rooting Watermarks in Latent Diffusion Models](https://openaccess.thecvf.com/content/ICCV2023/html/Fernandez_The_Stable_Signature_Rooting_Watermarks_in_Latent_Diffusion_Models_ICCV_2023_paper.html) · [arXiv](https://arxiv.org/abs/2303.15435) | ICCV | [Code](https://github.com/facebookresearch/stable_signature) | Fine-tunes a latent diffusion decoder to embed a fixed binary signature in generated images. |
| 2022 | [Responsible Disclosure of Generative Models Using Scalable Fingerprinting](https://openreview.net/forum?id=sOK-zS6WHB) · [arXiv](https://arxiv.org/abs/2012.08726) | ICLR | [Code](https://github.com/ningyu1991/ScalableGANFingerprints) | Scalable generation of fingerprinted GAN instances for source attribution. |


<a id="image-inference"></a>
### Diffusion sampling and latent-noise methods

Watermarks inserted through noise, latent variables, guidance, or sampling. Some methods also learn auxiliary components.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [FARI: Robust One-Step Inversion for Watermarking in Diffusion Models](https://proceedings.iclr.cc/paper_files/paper/2026/hash/dad66bb085bab14fbca07cfa4271f00b-Abstract-Conference.html) | ICLR | [Code](https://github.com/0xD009/FARI) | Efficient inversion for extracting diffusion watermarks. |
| 2026 | [GROW: Watermark Generation with Progressive Guidance for Diffusion Models](https://openaccess.thecvf.com/content/CVPR2026/html/Luo_GROW_Watermark_Generation_with_Progressive_Guidance_for_Diffusion_Models_CVPR_2026_paper.html) | CVPR | [Code](https://github.com/luopengchen/GROW) | Progressive guidance for watermark generation. |
| 2026 | [Guidance Watermarking for Diffusion Models](https://proceedings.iclr.cc/paper_files/paper/2026/hash/b9501301a7a0f99e57406cc901f4e3ce-Abstract-Conference.html) | ICLR | [Code](https://github.com/EnoalG/Guidance-Watermarking-for-Diffusion-Models) | Adds watermark guidance during diffusion sampling. |
| 2026 | [MaxMark: High-Capacity Diffusion-Native Watermarking via Robust and Invertible Latent Embedding](https://openaccess.thecvf.com/content/CVPR2026/html/Chang_MaxMark_High-Capacity_Diffusion-Native_Watermarking_via_Robust_and_Invertible_Latent_Embedding_CVPR_2026_paper.html) | CVPR | [Code](https://github.com/SeRAlab/MaxMark) | High-capacity watermarking through invertible latent embedding. |
| 2026 | [Spherical Watermark: Encryption-Free, Lossless Watermarking for Diffusion Models](https://proceedings.iclr.cc/paper_files/paper/2026/hash/bb97e9a7c811904c9b01f51fde66edcf-Abstract-Conference.html) | ICLR | — | Embeds bits through spherical mapping of the initial noise. |
| 2025 | [An Undetectable Watermark for Generative Image Models](https://arxiv.org/abs/2410.07369) | ICLR | [Code](https://github.com/XuandongZhao/PRC-Watermark) | PRC watermark: pseudorandom error-correcting codes in diffusion noise; security depends on the code construction and assumptions. |
| 2025 | [GaussMarker: Robust Dual-Domain Watermark for Diffusion Models](https://proceedings.mlr.press/v267/li25ae.html) | ICML | [Code](https://github.com/SunnierLee/GaussMarker) | Combines spatial and frequency-domain watermark signals in diffusion noise. |
| 2025 | [Hidden in the Noise: Two-Stage Robust Watermarking for Images](https://proceedings.iclr.cc/paper_files/paper/2025/hash/999fcab97007ebef0cda9949550b4a9e-Abstract-Conference.html) | ICLR | [Code](https://github.com/Kasraarabi/Hidden-in-the-Noise) | WIND: two-stage identification using watermarked diffusion noise. |
| 2025 | [SEAL: Semantic Aware Image Watermarking](https://openaccess.thecvf.com/content/ICCV2025/html/Arabi_SEAL_Semantic_Aware_Image_Watermarking_ICCV_2025_paper.html) · [arXiv](https://arxiv.org/abs/2503.12172) | ICCV | [Code](https://github.com/Kasraarabi/SEAL) | Semantic-aware embedding for image watermark robustness. |
| 2025 | [Semantic Watermarking Reinvented: Enhancing Robustness and Generation Quality with Fourier Integrity](https://arxiv.org/abs/2509.07647) | ICCV | [Code](https://github.com/thomas11809/SFWMark) | SFWMark: balances semantic watermark robustness and Fourier-domain integrity. |
| 2025 | [Shallow Diffuse: Robust and Invisible Watermarking through Low-Dimensional Subspaces in Diffusion Models](https://arxiv.org/abs/2410.21088) | NeurIPS | [Code](https://github.com/liwd190019/Shallow-Diffuse) | Embeds watermarks in low-dimensional diffusion subspaces. |
| 2024 | [A Training-Free Plug-and-Play Watermark Framework for Stable Diffusion](https://arxiv.org/abs/2404.05607) | Preprint | — | Training-free watermark insertion for Stable Diffusion. |
| 2024 | [DiffuseTrace: A Transparent and Flexible Watermarking Scheme for Latent Diffusion Model](https://arxiv.org/abs/2405.02696) | Preprint | [Code](https://github.com/REPO-EXP/DiffuseTrace) | Latent-space message embedding without fine-tuning the diffusion model. |
| 2024 | [Gaussian Shading: Provable Performance-Lossless Image Watermarking for Diffusion Models](https://arxiv.org/abs/2404.04956) | CVPR | [Code](https://github.com/bsmhmmlf/Gaussian-Shading) | Encodes payloads in initial noise while preserving its distribution under the scheme's assumptions. |
| 2024 | [RingID: Rethinking Tree-Ring Watermarking for Enhanced Multi-Key Identification](https://arxiv.org/abs/2404.14055) | ECCV | [Code](https://github.com/showlab/RingID) | Multi-key identification through redesigned Fourier-domain noise patterns. |
| 2024 | [ROBIN: Robust and Invisible Watermarks for Diffusion Models with Adversarial Optimization](https://arxiv.org/abs/2411.03862) | NeurIPS | [Code](https://github.com/Hannah1102/ROBIN) | Adversarial optimization of diffusion watermarks for visibility and robustness. |
| 2023 | [Tree-Rings Watermarks: Invisible Fingerprints for Diffusion Images](https://openreview.net/pdf?id=Z57JrmubNl) | NeurIPS | [Code](https://github.com/YuxinWenRick/tree-ring-watermark) | Tree-Ring: Fourier-domain patterns in the initial diffusion noise; detection by inversion. |


<a id="image-ar"></a>
### Autoregressive image generation

Discrete visual-token and bitwise image generation; retokenization robustness is a distinct challenge.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [ClusterMark: Towards Robust Watermarking for Autoregressive Image Generators with Visual Token Clustering](https://openaccess.thecvf.com/content/CVPR2026/html/Lukovnikov_ClusterMark_Towards_Robust_Watermarking_for_Autoregressive_Image_Generators_with_Visual_CVPR_2026_paper.html) | CVPR | [Code](https://github.com/lukovnikov/ClusterMark) | Clusters visually similar tokens to improve robustness after image transformations and retokenization. |
| 2026 | [UniMark: Unified Adaptive Multi-bit Watermarking for Autoregressive Image Generators](https://arxiv.org/abs/2604.11843) | Preprint | — | Adaptive multi-bit watermarking of autoregressive image generators. |
| 2025 | [BitMark: Watermarking Bitwise Autoregressive Image Generative Models](https://arxiv.org/abs/2506.21209) | Preprint | [Code](https://github.com/sprintml/BitMark) | Watermarking for bitwise autoregressive image generation. |
| 2025 | [Training-Free Watermarking for Autoregressive Image Generation](https://arxiv.org/abs/2505.14673) | Preprint | [Code](https://github.com/maifoundations/IndexMark) | IndexMark: watermarking discrete visual-token generation without additional training. |
| 2025 | [Watermarking Autoregressive Image Generation](https://proceedings.neurips.cc/paper_files/paper/2025/hash/67cef321196556c4f1e17989ed7a8adb-Abstract-Conference.html) | NeurIPS | [Code](https://github.com/facebookresearch/wmar) | WMAR: token watermarking with tokenizer adaptation and synchronization; includes an audio study. |


<a id="image-post"></a>
### Post-generation image watermarks and foundations

Methods applicable to completed images, including learned embedding, steganography, tamper localization, and defenses for watermark extractors.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2025 | [Decoder Gradient Shield: Provable and High-Fidelity Prevention of Gradient-Based Box-Free Watermark Removal](https://openaccess.thecvf.com/content/CVPR2025/html/An_Decoder_Gradient_Shield_Provable_and_High-Fidelity_Prevention_of_Gradient-Based_Box-Free_CVPR_2025_paper.html) | CVPR | [Code](https://github.com/haonanAN309/CVPR-2025-Official-Implementation-Decoder-Gradient-Shield) | Protects a watermark decoder against gradient-based training of a removal network. |
| 2025 | [InvisMark: Invisible and Robust Watermarking for AI-Generated Image Provenance](https://openaccess.thecvf.com/content/WACV2025/html/Xu_InvisMark_Invisible_and_Robust_Watermarking_for_AI-Generated_Image_Provenance_WACV_2025_paper.html) | WACV | [Code](https://github.com/microsoft/InvisMark) | High-resolution image watermarking for provenance payloads. |
| 2025 | [Pixel Seal: Adversarial-only Training for Invisible Image and Video Watermarking](https://arxiv.org/abs/2512.16874) | Preprint | [Code](https://github.com/facebookresearch/videoseal) | Adversarial training for post-generation image and video watermarks. |
| 2025 | [Robust Watermarking Using Generative Priors Against Image Editing: From Benchmarking to Advances](https://openreview.net/forum?id=16O8GCm8Wn) · [arXiv](https://arxiv.org/abs/2410.18775) | ICLR | [Code](https://github.com/Shilin-LU/VINE) | VINE: watermarking using generative priors, with evaluation under diffusion-based image editing. |
| 2025 | [TrustMark: Robust Watermarking and Watermark Removal for Arbitrary Resolution Images](https://openaccess.thecvf.com/content/ICCV2025/html/Bui_TrustMark_Robust_Watermarking_and_Watermark_Removal_for_Arbitrary_Resolution_Images_ICCV_2025_paper.html) | ICCV | [Code](https://github.com/adobe/trustmark) | Arbitrary-resolution image watermarking and a learned removal component. |
| 2025 | [Watermark Anything with Localized Messages](https://proceedings.iclr.cc/paper_files/paper/2025/hash/c5ee1911dffe4886367eee7fca314235-Abstract-Conference.html) | ICLR | [Code](https://github.com/facebookresearch/watermark-anything) | WAM: localized, multi-message image watermarking. Repository archived; code remains available. |
| 2025 | [We Can Hide More Bits: The Unused Watermarking Capacity in Theory and in Practice](https://arxiv.org/abs/2510.12812) | Preprint | [Code](https://github.com/facebookresearch/videoseal) | ChunkySeal: watermark payload capacity and high-capacity image/video embedding. |
| 2024 | [Attack-Resilient Image Watermarking Using Stable Diffusion](https://arxiv.org/abs/2401.04247) | NeurIPS | [Code](https://github.com/zhanglijun95/ZoDiac) | ZoDiac: optimizes a latent watermark for an existing image using Stable Diffusion. |
| 2024 | [EditGuard: Versatile Image Watermarking for Tamper Localization and Copyright Protection](https://openaccess.thecvf.com/content/CVPR2024/papers/Zhang_EditGuard_Versatile_Image_Watermarking_for_Tamper_Localization_and_Copyright_Protection_CVPR_2024_paper.pdf) · [arXiv](https://arxiv.org/abs/2312.08883) | CVPR | [Code](https://github.com/xuanyuzhang21/EditGuard) | Joint payload recovery and tamper localization. |
| 2024 | [RAW: A Robust and Agile Plug-and-Play Watermark Framework for AI-Generated Images with Provable Guarantees](https://papers.nips.cc/paper_files/paper/2024/hash/ee62ab636066cf45a27246acca9545b7-Abstract-Conference.html) | NeurIPS | [Code](https://github.com/jeremyxianx/RAWatermark) | Learnable image-space watermarks with a calibrated detection framework. |
| 2024 | [WaterDiff: Perceptual Image Watermarks Via Diffusion Model](https://ieeexplore.ieee.org/abstract/document/10447095) | ICASSP | — | Uses a diffusion model for perceptual image watermark embedding. |
| 2023 | [A Somewhat Robust Image Watermark against Diffusion-based Editing Models](https://arxiv.org/abs/2311.13713) | Preprint | [Code](https://github.com/BennyTMT/RIW) | RIW: watermark embedding designed to withstand diffusion-based image edits. |
| 2023 | [Flow-Based Robust Watermarking with Invertible Noise Layer for Black-Box Distortions](https://ojs.aaai.org/index.php/AAAI/article/view/25633) | AAAI | [Code](https://github.com/QQiuyp/FIN) | FIN: invertible noise modeling for robustness to black-box image distortions. |
| 2023 | [Practical Deep Dispersed Watermarking with Synchronization and Fusion](https://dl.acm.org/doi/10.1145/3581783.3612015) · [arXiv](https://arxiv.org/abs/2310.14532) | ACM MM | [Code](https://github.com/bytedance/DWSF) | DWSF: dispersed embedding, synchronization, and message fusion for arbitrary-resolution images. Repository archived; code remains available. |
| 2023 | [RoSteALS: Robust Steganography using Autoencoder Latent Space](https://openaccess.thecvf.com/content/CVPR2023W/WMF/html/Bui_RoSteALS_Robust_Steganography_Using_Autoencoder_Latent_Space_CVPRW_2023_paper.html) · [arXiv](https://arxiv.org/abs/2304.03400) | CVPR Workshops (CVPRW) | [Code](https://github.com/TuBui/RoSteALS) | Learned steganography using a pretrained autoencoder latent space; applicable after generation. |
| 2022 | [Enhancing the Robustness of Deep Learning Based Fingerprinting to Improve Deepfake Attribution](https://dl.acm.org/doi/abs/10.1145/3551626.3564981) | ACM MM-Asia | — | Studies distortion-robust learned fingerprints for deepfake attribution. |
| 2022 | [Towards Blind Watermarking: Combining Invertible and Non-invertible Mechanisms](https://dl.acm.org/doi/abs/10.1145/3503161.3547950) · [arXiv](https://arxiv.org/abs/2212.12678) | ACM MM | [Code](https://github.com/rmpku/CIN) | CIN: combines invertible and non-invertible components for blind image watermarking. |
| 2022 | [Watermarking Images in Self-Supervised Latent Spaces](https://ieeexplore.ieee.org/document/9746058) · [arXiv](https://arxiv.org/abs/2112.09581) | ICASSP | [Code](https://github.com/facebookresearch/ssl_watermarking) | Optimizes watermarks in a self-supervised feature space; supports detection and payloads. |
| 2020 | [StegaStamp: Invisible Hyperlinks in Physical Photographs](https://ieeexplore.ieee.org/document/9156548) · [arXiv](https://arxiv.org/abs/1904.05343) | CVPR | [Code](https://github.com/tancik/StegaStamp) | Learned image watermarks designed to survive physical printing and camera capture. |
| 2018 | [HiDDeN: Hiding Data With Deep Networks](https://dl.acm.org/doi/abs/10.1007/978-3-030-01267-0_40) · [arXiv](https://arxiv.org/abs/1807.09937) | ECCV | [Code](https://github.com/jirenz/HiDDeN) | Foundational learned encoder-decoder watermarking with simulated distortions. |


## Video

Includes direct video watermarking, watermarking during video generation, and image watermarks designed to persist through image-to-video generation. See also Pixel Seal and ChunkySeal in [image methods](#image-post).

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [LoT-Pass: Long-term-robust Image Watermarking for Image to Video Generation](https://arxiv.org/abs/2509.17773) | ECCV | [Code](https://github.com/MrCrims/LoT-Pass-Long-term-robust-Image-Watermarking-for-Image-to-Video-Generation) | Watermarks an input image to retain recoverable signals after image-to-video generation; formerly I2VWM. |
| 2026 | [SIGMark: Scalable In-Generation Watermark with Blind Extraction for Video Diffusion](https://proceedings.iclr.cc/paper_files/paper/2026/hash/f3f6f1739b646e0bd20111261ce23adb-Abstract-Conference.html) | ICLR | [Code](https://github.com/JeremyZhao1998/SIGMark-release) | Scalable video generation watermarking with blind message extraction. |
| 2026 | [SPDMark: Selective Parameter Displacement for Robust Video Watermarking](https://openaccess.thecvf.com/content/CVPR2026/html/Fares_SPDMark_Selective_Parameter_Displacement_for_Robust_Video_Watermarking_CVPR_2026_paper.html) | CVPR | [Code](https://github.com/Samar-Fares/SPDMark) | Selective parameter displacement in video generators, with frame-specific messages for tamper localization. |
| 2026 | [WaTeRFlow: Watermark Temporal Robustness via Flow Consistency](https://openaccess.thecvf.com/content/CVPR2026/html/Jeong_WaTeRFlow_Watermark_Temporal_Robustness_via_Flow_Consistency_CVPR_2026_paper.html) | CVPR | [Code](https://github.com/kuai-lab/cvpr26_WaTeRFlow) | Watermarks input images to preserve detection after image-to-video generation using flow consistency. |
| 2025 | [VideoMark: A Distortion-Free Robust Watermarking Framework for Video Diffusion Models](https://arxiv.org/abs/2504.16359) | Preprint | [Code](https://github.com/KYRIE-LI11/VideoMark) | Watermarking the noise used by video diffusion models. |
| 2025 | [VideoShield: Regulating Diffusion-based Video Generation Models via Watermarking](https://proceedings.iclr.cc/paper_files/paper/2025/hash/8227285e32f70e07fa3a247f3a48006d-Abstract-Conference.html) | ICLR | [Code](https://github.com/hurunyi/VideoShield) | In-generation video watermarking for attribution and tamper detection. |
| 2024 | [Video Seal: Open and Efficient Video Watermarking](https://arxiv.org/abs/2412.09492) | Preprint | [Code](https://github.com/facebookresearch/videoseal) | Post-generation video watermarking with temporal propagation and open training code. |
| 2023 | [A Novel Deep Video Watermarking Framework with Enhanced Robustness to H.264/AVC Compression](https://dl.acm.org/doi/10.1145/3581783.3612270) | ACM MM | [Code](https://github.com/yulinsysu/REVMark) | REVMark: learned video watermarking with H.264/AVC compression robustness. |
| 2019 | [Robust Invisible Video Watermarking with Attention](https://arxiv.org/abs/1909.01285) | Preprint | [Code](https://github.com/DAI-Lab/RivaGAN) | RivaGAN: foundational learned post-generation video watermarking baseline. |


<a id="audio"></a>
## Speech, audio, and music

Scope includes voice cloning, TTS, neural codecs, audio diffusion, and autoregressive audio generation. Reference-speech watermark transfer is also covered under [training-data attribution](#data).


<a id="audio-post"></a>
### Waveform and codec-based embedding

Embeds into existing audio or its intermediate representation; applicability after generation does not imply that an adversarial generator must use the embedder.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [Latent-Mark: An Audio Watermark Robust to Neural Codec Compression](https://arxiv.org/abs/2603.05310) | Interspeech | [Code](https://github.com/yenshan0530/Latent-Mark) | Optimizes waveform perturbations against codec-latent detection objectives; tests transfer across neural codecs. |
| 2026 | [NeuMark: Neural Codec Resynthesis-Robust Audio Watermarking in the Codec Latent Space](https://arxiv.org/abs/2609.25719) | Preprint | — | Codec-latent watermarking targeting neural resynthesis robustness. |
| 2025 | [AudioMarkNet: Audio Watermarking for Deepfake Speech Detection](https://www.usenix.org/system/files/usenixsecurity25-zong.pdf) | USENIX Security | [Artifact](https://zenodo.org/records/14722182) | Neural audio watermarking for proactive deepfake speech detection. |
| 2025 | [AWARE: Audio Watermarking with Adversarial Resistance to Edits](https://arxiv.org/abs/2510.17512) | Preprint | [Code](https://github.com/deepmark/aware) | Adversarial training for robust audio payload recovery. |
| 2025 | [Speech Watermarking with Discrete Intermediate Representations](https://arxiv.org/abs/2412.13917) | AAAI | [Audio samples](https://discretewm.github.io/discrete_wm/) | DiscreteWM: maps speech to a discrete representation and modifies token IDs to embed watermark messages. |
| 2025 | [XAttnMark: Learning Robust Audio Watermarking with Cross-Attention](https://proceedings.mlr.press/v267/liu25ap.html) | ICML | [Project / samples](https://liuyixin-louis.github.io/xattnmark/) | Cross-attention for message retrieval and robustness to generative audio editing. |
| 2024 | [MaskMark: Robust Neural Watermarking for Real and Synthetic Speech](https://interactiveaudiolab.github.io/assets/papers/oreilly_jin_su_pardo_watermark.pdf) · [PDF](https://ieeexplore.ieee.org/ielx7/10445798/10445803/10447253.pdf) | ICASSP | [Project](https://interactiveaudiolab.github.io/project/maskmark.html) | MaskMark: neural watermarking of real and synthetic speech. |
| 2024 | [Proactive Detection of Voice Cloning with Localized Watermarking](https://arxiv.org/abs/2401.17264) | ICML | [Code](https://github.com/facebookresearch/audioseal) | AudioSeal: localized watermark detection and optional message attribution for speech. |
| 2024 | [SilentCipher: Deep Audio Watermarking](https://www.isca-archive.org/interspeech_2024/singh24_interspeech.pdf) | Interspeech | [Code](https://github.com/sony/silentcipher) | Neural audio watermarking with public models and inference code. |
| 2023 | [AudioQR: Deep Neural Audio Watermarks For QR Code](https://www.ijcai.org/proceedings/2023/687) | IJCAI | [Code](https://github.com/xinghua-qu/AudioQR) | Encodes recoverable QR-code information into audio; adjacent audio data-hiding application. |
| 2023 | [WavMark: Watermarking for Audio Generation](https://arxiv.org/abs/2308.12770) | Preprint | [Code](https://github.com/wavmark/wavmark) | Invertible-network audio watermarking with synchronization and binary message recovery. |


<a id="audio-model"></a>
### Speech and audio model integration

Watermarking integrated into synthesis models, codec training, or their training pipelines.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [MusicMark: A Robust Generative Watermarking Framework for Music Generation](https://arxiv.org/abs/2607.11117) | Under review (preprint) | — | Embeds watermarks into semantic latents during generation using an adapter on a frozen diffusion backbone; reports robustness to neural-codec resynthesis and cover-song attacks. |
| 2026 | [NeuMark-Native: Robust Text-to-Speech-Native Watermarking Through Full Utilization of Neural Audio Codec Latent Space](https://arxiv.org/abs/2610.05215) | Preprint | — | Adapts watermark modules to TTS-generated codec tokens while freezing the TTS model and codec. |
| 2025 | [Latent Watermarking of Audio Generative Models](https://ieeexplore.ieee.org/document/10889782/) | ICASSP | — | Watermarking designed to persist through audio-codec latent representations and generation. |
| 2025 | [P2Mark: Plug-and-play Parameter-intrinsic Watermarking for Neural Speech Generation](https://arxiv.org/abs/2504.05197) | Preprint | — | Embeds watermarks through speech-generator parameters. |
| 2025 | [Poisoning The Diffusion: A Simple and Robust Watermarking Method for Audio Generation](https://ieeexplore.ieee.org/document/10889187/) | ICASSP | — | Training-stage watermarking of diffusion audio generation. |
| 2025 | [WMCodec: End-to-End Neural Speech Codec with Deep Watermarking for Authenticity Verification](https://ieeexplore.ieee.org/document/10888430) · [arXiv](https://arxiv.org/abs/2409.12121) | ICASSP | [Code](https://github.com/zjzser/WMCodec) | Joint neural speech coding and deep watermarking for authenticity verification. |
| 2024 | [Collaborative Watermarking for Adversarial Speech Synthesis](https://arxiv.org/abs/2309.15224) | ICASSP | — | Collaborative embedding for speech synthesis, evaluated under adversarial conditions. |
| 2024 | [GROOT: Generating Robust Watermark for Diffusion-Model-Based Audio Synthesis](https://dl.acm.org/doi/10.1145/3664647.3680596) · [arXiv](https://arxiv.org/abs/2407.10471) | ACM MM | [Code](https://github.com/Groot-GAW/Groot) | Fine-tunes diffusion-based audio synthesis for native watermark generation. |
| 2024 | [TraceableSpeech: Towards Proactively Traceable Text-to-Speech with Watermarking](https://arxiv.org/abs/2406.04840) | Preprint | [Code](https://github.com/zjzser/TraceableSpeech) | Integrates watermarking into TTS. Released repository covers the speech-watermark training component. |


<a id="audio-inference"></a>
### Autoregressive audio sampling

Sampling-based watermarks for discrete audio generation.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [Hidden in Plain Tokens: Simply Robust, Gradient-Free Watermark for Synthetic Audio](https://proceedings.mlr.press/v306/milis26a.html) | ICML | [Code](https://github.com/g-milis/nograd-audio-wm) | Clusters audio tokens to improve watermark robustness to codec round trips and edits. |
| 2025 | [Robust Distortion-Free Watermark for Autoregressive Audio Generation Models](https://arxiv.org/abs/2510.21115) | NeurIPS | [Code](https://github.com/g-milis/AlignedIS) | Aligned-IS: distribution-preserving sampling for autoregressive audio. |


<a id="text"></a>
## Text and language models

Methods span token sampling, sentence-level decisions, post-generation rewriting, model adaptation, and diffusion language generation.


<a id="text-token"></a>
### Token sampling and adaptive watermarking

Changes token selection or detection statistics, often using keyed vocabulary partitions or distribution-preserving sampling.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [SpARK: An Embarrassingly Simple Sparse Watermarking in LLMs with Enhanced Text Quality](https://aclanthology.org/2026.findings-eacl.240/) | Findings of EACL | [Code](https://github.com/mail-research/SpARK-llm-watermarking) | Sparse watermark insertion to reduce the effect on text quality. |
| 2025 | [An End-to-End Model for Logits-Based Large Language Models Watermarking](https://proceedings.mlr.press/v267/wong25a.html) | ICML | [Code](https://github.com/KahimWong/E2E-LLM-Watermark) | Learns watermark generation and detection jointly for logits-based insertion. |
| 2025 | [Enhancing LLM Watermark Resilience Against Both Scrubbing and Spoofing Attacks](https://proceedings.neurips.cc/paper_files/paper/2025/hash/2847043899e1171183ceadf86bdbb280-Abstract-Conference.html) | NeurIPS | — | SEEK: equivalent texture keys and sub-vocabulary decomposition. |
| 2025 | [Improved Unbiased Watermark for Large Language Models](https://aclanthology.org/2025.acl-long.1005/) | ACL | [Code](https://github.com/RayRuiboChen/MCMark) | MCmark: multi-channel vocabulary partitioning for unbiased watermarking. |
| 2025 | [Learning to Watermark: A Selective Watermarking Framework for Large Language Models via Multi-Objective Optimization](https://proceedings.neurips.cc/paper_files/paper/2025/hash/bc222e8153a49c1b30a1b8ba96b35117-Abstract-Conference.html) | NeurIPS | [Code](https://github.com/fattyray/learning-to-watermark) | LTW: a learned decision about when to watermark, balancing detection and text quality. |
| 2025 | [MorphMark: Flexible Adaptive Watermarking for Large Language Models](https://aclanthology.org/2025.acl-long.240/) | ACL | — | Adaptively adjusts watermark strength to the generation-quality/detectability trade-off. |
| 2025 | [Permute-and-Flip: An optimally stable and watermarkable decoder for LLMs](https://proceedings.iclr.cc/paper_files/paper/2025/hash/50be7e77b9c883144940be925b608acc-Abstract-Conference.html) | ICLR | [Code](https://github.com/XuandongZhao/pf-decoding) | PF decoding and an associated distortion-free watermark. |
| 2025 | [Watermarking Large Language Models: An Unbiased and Low-risk Method](https://aclanthology.org/2025.acl-long.391/) | ACL | [Code](https://github.com/djwei96/STA) | STA-1: sample-then-accept watermarking, including low-entropy generation. |
| 2024 | [Adaptive Text Watermark for Large Language Models](https://proceedings.mlr.press/v235/liu24e.html) | ICML | [Code](https://github.com/yepengliu/adaptive-text-watermark) | Adapts watermark strength to semantic and entropy information. |
| 2024 | [An Entropy-based Text Watermarking Detection Method](https://arxiv.org/abs/2403.13485) | ACL | [Code](https://github.com/luyijian3/EWD) | EWD: entropy-aware detection of watermarked text. |
| 2024 | [DiPmark: A Resilient and Accessible Distribution-Preserving Watermark for Large Language Models](https://arxiv.org/abs/2310.07710) | ICML | [Code](https://github.com/yihwu/DiPmark) | Distribution-preserving token reweighting with model-independent detection. |
| 2024 | [Duwak: Dual Watermarks in Large Language Models](https://aclanthology.org/2024.findings-acl.678.pdf) | Findings of ACL | [Code](https://github.com/chaoyitud/Dual-Watermarks) | Combines token-probability and sampling-pattern watermarks. |
| 2024 | [On the Reliability of Watermarks for Large Language Models](https://arxiv.org/abs/2306.04634) | ICLR | [Code](https://github.com/jwkirchenbauer/lm-watermarking) | Studies detection after paraphrasing, human editing, and mixing watermarked text into longer documents. |
| 2024 | [Provable Robust Watermarking for AI-Generated Text](https://openreview.net/forum?id=SsmT8aO45L) · [arXiv](https://arxiv.org/abs/2306.17439) | ICLR | [Code](https://github.com/XuandongZhao/Unigram-Watermark) | Unigram watermark with robustness guarantees under its edit model. |
| 2024 | [Robust Distortion-free Watermarks for Language Models](https://arxiv.org/abs/2307.15593) | TMLR | [Code](https://github.com/jthickstun/watermark) | KTH: distortion-free generation with robust sequence-alignment detection. |
| 2024 | [Scalable watermarking for identifying large language model outputs](https://www.nature.com/articles/s41586-024-08025-4) | Nature | [Code](https://github.com/google-deepmind/synthid-text) | SynthID-Text: tournament-based token sampling and scalable detection. |
| 2024 | [Token-Specific Watermarking with Enhanced Detectability and Semantic Coherence for Large Language Models](https://proceedings.mlr.press/v235/huo24a.html) · [arXiv](https://arxiv.org/abs/2402.18059) | ICML | [Code](https://github.com/mignonjia/TS_watermark) | Token-dependent watermark strengths balance detection and semantic coherence. Repository archived. |
| 2024 | [Unbiased Watermark for Large Language Models](https://arxiv.org/abs/2310.10669) | ICLR | [Code](https://github.com/xiaoniu-578fa6bff964d005/UnbiasedWatermark) | Distribution-preserving watermarking and robust likelihood-ratio detection. |
| 2023 | [A Watermark for Large Language Models](https://arxiv.org/abs/2301.10226) | ICML | [Code](https://github.com/jwkirchenbauer/lm-watermarking) | KGW: keyed red/green token lists and statistical detection. |


<a id="text-semantic"></a>
### Semantic, sentence, and prompt-based methods

Uses semantics, sentences, topics, or prompts; some methods still intervene in token generation.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [In-Context Watermarks for Large Language Models](https://proceedings.iclr.cc/paper_files/paper/2026/hash/571082ea18d30060177dfcaf662ff0e5-Abstract-Conference.html) | ICLR | [Code](https://github.com/yepengliu/In-Context-Watermarks) | Prompt-based watermarking through in-context learning and instruction following. |
| 2026 | [PASA: A Principled Embedding-Space Watermarking Approach for LLM-Generated Text under Semantic-Invariant Attacks](https://proceedings.mlr.press/v306/ai26b.html) | ICML | [Code](https://github.com/ai-kunkun/PASA) | Embedding-space watermarking designed for semantic-preserving transformations. |
| 2026 | [Topic-Based Watermarks for Large Language Models](https://aclanthology.org/2026.findings-acl.1220/) | Findings of ACL | — | Topic-aligned vocabulary subsets for text watermarking. |
| 2025 | [SimMark: A Robust Sentence-Level Similarity-Based Watermarking Algorithm for Large Language Models](https://aclanthology.org/2025.emnlp-main.1567/) | EMNLP | [Code](https://github.com/DabiriAghdam/SimMark) | Sentence embedding similarity and rejection sampling; does not require model logits. |
| 2025 | [WaterPool: A Language Model Watermark Mitigating Trade-Offs among Imperceptibility, Efficacy and Robustness](https://aclanthology.org/2025.naacl-long.209/) | NAACL | [Code](https://github.com/skpig/waterpool) | Semantic retrieval for restoring watermark keys at detection time. |
| 2024 | [A Semantic Invariant Robust Watermark for Large Language Models](https://arxiv.org/abs/2310.06356) | ICLR | [Code](https://github.com/THU-BPM/Robust_Watermark) | SIR: semantic-dependent watermark signals designed for paraphrase robustness. |
| 2024 | [k-SemStamp: A Clustering-Based Semantic Watermark for Detection of Machine-Generated Text](https://aclanthology.org/2024.findings-acl.98/) | Findings of ACL | [Code](https://github.com/abehou/SemStamp) | Clusters semantic embeddings for sentence-level watermarking. |
| 2024 | [SemStamp: A Semantic Watermark with Paraphrastic Robustness for Text Generation](https://aclanthology.org/2024.naacl-long.226/) | NAACL | [Code](https://github.com/abehou/SemStamp) | Sentence-level rejection sampling in semantic embedding space. |


<a id="text-post"></a>
### Post-generation text watermarking

Rewrites or transforms existing text to embed a detectable signal or payload.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2025 | [Robust Multi-bit Text Watermark with LLM-based Paraphrasers](https://proceedings.mlr.press/v267/xu25k.html) | ICML | [Code](https://github.com/xiaojunxu/multi-bit-text-watermark) | Pairs of fine-tuned paraphrasers encode multi-bit payloads through sentence-level differences. |
| 2024 | [PostMark: A Robust Blackbox Watermark for Large Language Models](https://aclanthology.org/2024.emnlp-main.506/) | EMNLP | [Code](https://github.com/lilakk/PostMark) | Post-generation insertion of words selected through a semantic embedding. |
| 2021 | [Adversarial Watermarking Transformer: Towards Tracing Text Provenance with Data Hiding](https://ieeexplore.ieee.org/document/9519400) · [arXiv](https://arxiv.org/abs/2009.03015) | IEEE S&P | [Code](https://github.com/S-Abdelnabi/awt) | AWT: neural text rewriting for hidden-message embedding and provenance tracing. |


<a id="text-multibit"></a>
### Multi-bit messages and attribution

Methods emphasizing payload recovery, attribution, or selective disclosure. Multi-bit capability also appears in other sections.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [Selective Disclosure Watermarking for Large Language Models](https://proceedings.mlr.press/v306/chen26bs.html) | ICML | [Code](https://github.com/xuyangc03/hero-watermark) | Verifies selected parts of an embedded message without revealing the entire payload. |
| 2025 | [Provably Robust Multi-bit Watermarking for AI-generated Text](https://www.usenix.org/system/files/usenixsecurity25-qu-watermarking.pdf) | USENIX Security | [Code](https://github.com/randomizedtree/segment-watermark) | Multi-bit text attribution using pseudorandom segment assignment and robustness-enhancing coding. |
| 2025 | [StealthInk: A Multi-bit and Stealthy Watermark for Large Language Models](https://proceedings.mlr.press/v267/jiang25j.html) | ICML | [Repo placeholder](https://github.com/yajiang4215/StealthInk_A-Multi-bit-and-Stealthy-Watermark-for-Large-Language-Models) | Multi-bit attribution with a focus on watermark stealthiness; linked repository is a release placeholder. |
| 2024 | [Towards Codable Watermarking for Injecting Multi-Bits Information to LLMs](https://proceedings.iclr.cc/paper_files/paper/2024/hash/abdc8c031aa6c6917c3b593166e5e340-Abstract-Conference.html) · [arXiv](https://arxiv.org/abs/2307.15992) | ICLR | [Code](https://github.com/lancopku/codable-watermarking-for-llm) | Encodes recoverable messages through language-model token generation. |


<a id="text-public"></a>
### Public verification and spoofing resistance

Separates public verification from possession of an embedding secret. Consult each paper for its specific security definition.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2025 | [Publicly-Detectable Watermarking for Language Models](https://cic.iacr.org/p/1/4/31/pdf) | IACR Communications in Cryptology | [Code](https://github.com/jfairoze/publicly-detectable-watermark) | Cryptographic public detection using signature-based constructions. |
| 2024 | [An Unforgeable Publicly Verifiable Watermark for Large Language Models](https://proceedings.iclr.cc/paper_files/paper/2024/hash/214d2cffc381938be6f7254d5382904f-Abstract-Conference.html) | ICLR | [Code](https://github.com/THU-BPM/unforgeable_watermark) | UPV: learned asymmetry between watermark generation and public detection. |
| 2024 | [Bileve: Securing Text Provenance in Large Language Models Against Spoofing with Bi-level Signature](https://arxiv.org/abs/2406.01946) | NeurIPS | [Code](https://github.com/Tongzhou0101/Bileve-official) | Combines a fine-grained signature and coarse-grained watermark for provenance and spoofing analysis. |


<a id="text-model"></a>
### Watermarks learned or embedded in model weights

Watermarked outputs arise from model adaptation rather than a mandatory custom sampler alone.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2025 | [GaussMark: A Practical Approach for Structural Watermarking of Language Models](https://proceedings.mlr.press/v267/block25a.html) | ICML | [Code](https://github.com/abblock/gaussmark) | Gaussian perturbations to model weights produce statistically detectable output signals. |
| 2024 | [On the Learnability of Watermarks for Language Models](https://arxiv.org/abs/2312.04469) | ICLR | [Code](https://github.com/chenchenygu/watermark-learnability) | Distills decoding-time watermarks into model weights; evaluates fine-tuning fragility. |


<a id="text-diffusion"></a>
### Diffusion language models

Methods designed for iterative or non-autoregressive text generation.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [From TDMA to CDMA: A Multi-bit Watermark for Diffusion Language Models](https://aclanthology.org/2026.acl-long.1737/) | ACL | — | Code-division multi-bit embedding for diffusion language models. |
| 2026 | [LR-DWM: Efficient Watermarking for Diffusion Language Models](https://aclanthology.org/2026.findings-acl.2161/) | Findings of ACL | — | Efficient watermarking tailored to diffusion language generation. |
| 2026 | [Watermarking Diffusion Language Models](https://proceedings.iclr.cc/paper_files/paper/2026/hash/a30d1f9de116aa3184e66a3e9d96b69d-Abstract-Conference.html) | ICLR | [Code](https://github.com/eth-sri/diffusion-lm-watermark) | Watermarks for non-autoregressive diffusion text generation. |
| 2026 | [You Can Have a Second Chance: Unbiased and Multi-bit Watermarking for Diffusion Language Models with Regret-based Remasking](https://aclanthology.org/2026.acl-long.1297/) | ACL | [Code](https://github.com/iieSKLCSDsg/UMR) | UMR: remasking-based unbiased and multi-bit diffusion-LM watermarking. |


<a id="code"></a>
## Source code

Token-based, training-based, and semantics-preserving post-generation methods. Correctness and resistance to refactoring require separate evaluation.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [Efficient and Universal Watermarking for LLM-Generated Code Detection](https://arxiv.org/abs/2402.07518) | [IEEE TSE](https://ink.library.smu.edu.sg/sis_research/11210/) | [Code](https://github.com/boutiquelee/ACW) | ACW: semantics-preserving code transformations and error correction; formerly Resilient Watermarking for LLM-Generated Codes. |
| 2026 | [Robust and Secure Code Watermarking for Large Language Models via ML/Crypto Codesign](https://arxiv.org/abs/2502.02068) | ACM TAISAP (accepted; author-reported) | — | RoSeMary: neural code transformation with zero-knowledge watermark verification; acceptance reported on arXiv v3. |
| 2025 | [Practical and Effective Code Watermarking for Large Language Models](https://proceedings.neurips.cc/paper_files/paper/2025/hash/6b4cb812f234a92ab757d1544912b4a8-Abstract-Conference.html) | NeurIPS | [Code](https://github.com/TimeLovercc/code-watermark) | Model training embeds code watermarks while accounting for code-generation constraints. |
| 2024 | [SrcMarker: Dual-Channel Source Code Watermarking via Scalable Code Transformations](https://ieeexplore.ieee.org/document/10646683/) | IEEE S&P | [Code](https://github.com/YBRua/SrcMarker) | Post-generation code transformation; preserves program semantics while embedding identifiers. |
| 2024 | [Who Wrote this Code? Watermarking for Code Generation](https://aclanthology.org/2024.acl-long.268/) | ACL | [Code](https://github.com/hongcheki/sweet-watermark) | SWEET: entropy-thresholded token watermarking for low-entropy source code. |


<a id="3d"></a>
## 3D scenes and neural rendering

Watermarks in NeRF and Gaussian-splat assets or their rendered views. These include scene reconstruction and representation protection, not only text-to-3D generation.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [Write Where It Matters: Policy-Guided Watermarks for 3D Gaussian Splatting](https://openaccess.thecvf.com/content/CVPR2026/papers/Li_Write_Where_It_Matters_Policy-Guided_Watermarks_for_3D_Gaussian_Splatting_CVPR_2026_paper.pdf) | CVPR | — | Policy-guided placement of watermarks in 3D Gaussian representations. |
| 2025 | [3D-GSW: 3D Gaussian Splatting for Robust Watermarking](https://openaccess.thecvf.com/content/CVPR2025/html/Jang_3D-GSW_3D_Gaussian_Splatting_for_Robust_Watermarking_CVPR_2025_paper.html) | CVPR | [Code](https://github.com/kuai-lab/cvpr25_3D-GSW) | Protects both 3D Gaussian assets and rendered images. |
| 2025 | [GuardSplat: Efficient and Robust Watermarking for 3D Gaussian Splatting](https://openaccess.thecvf.com/content/CVPR2025/papers/Chen_GuardSplat_Efficient_and_Robust_Watermarking_for_3D_Gaussian_Splatting_CVPR_2025_paper.pdf) | CVPR | [Code](https://github.com/NarcissusEx/GuardSplat) | Efficient, robust watermarking of Gaussian-splat scene representations. |
| 2024 | [GaussianMarker: Uncertainty-Aware Copyright Protection of 3D Gaussian Splatting](https://arxiv.org/abs/2410.23718) | NeurIPS | [Code](https://github.com/kevinhuangxf/GaussianMarker) | Watermarks 3D Gaussian representations and their rendered views. |
| 2024 | [WateRF: Robust Watermarks in Radiance Fields for Protection of Copyrights](https://arxiv.org/abs/2405.02066) | CVPR | [Code](https://github.com/kuai-lab/cvpr2024_WateRF) | Fine-tunes implicit and explicit radiance fields using wavelet-domain watermark objectives. |
| 2023 | [CopyRNeRF: Protecting the CopyRight of Neural Radiance Fields](https://arxiv.org/abs/2307.11526) | ICCV | [Code](https://github.com/luo-ziyuan/CopyRNeRF-code) | Embeds recoverable messages into NeRF-rendered images. |


<a id="ownership"></a>
## Training-data attribution and model ownership

These methods answer whether a model used particular data or whether a queried model carries an ownership mark; this differs from authenticating every output.


<a id="data"></a>
### Training-data attribution and watermark transfer

Includes deliberate marking of training examples or reference media and measurement of watermark inheritance.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2025 | [DiffusionShield: A Watermark for Data Copyright Protection against Generative Diffusion Models](https://dl.acm.org/doi/10.1145/3715073.3715079) · [arXiv](https://arxiv.org/abs/2306.04642) | SIGKDD Explorations | [Code](https://github.com/Yingqiancui/DiffusionShield) | Marks training images so their use by a diffusion model can be traced through generated outputs. |
| 2025 | [FT-Shield: A Watermark Against Unauthorized Fine-tuning in Text-to-Image Diffusion Models](https://doi.org/10.1145/3715073.3715080) · [arXiv](https://arxiv.org/abs/2310.02401) | SIGKDD Explorations | [Code](https://github.com/Yingqiancui/FT-Shield) | Watermarks training images for fine-tuning attribution, with mixture-of-experts detection across fine-tuning methods. |
| 2025 | [Harnessing Frequency Spectrum Insights for Image Copyright Protection Against Diffusion Models](https://openaccess.thecvf.com/content/CVPR2025/html/Liu_Harnessing_Frequency_Spectrum_Insights_for_Image_Copyright_Protection_Against_Diffusion_CVPR_2025_paper.html) | CVPR | [Code](https://github.com/sccsok/CoprGuard) | CoprGuard: frequency-domain watermarks for tracing unauthorized training and fine-tuning on images. |
| 2025 | [Radioactive Watermarks in Diffusion and Autoregressive Image Generative Models](https://arxiv.org/abs/2506.23731) | Preprint | [Code](https://github.com/sprintml/WIAR) | Studies whether image watermarks transfer when generated images train another model. |
| 2025 | [Robust Data Watermarking in Language Models by Injecting Fictitious Knowledge](https://aclanthology.org/2025.findings-acl.736/) | Findings of ACL | — | Training-data attribution through inserted fictitious facts and subsequent model queries. |
| 2024 | [Detecting Voice Cloning Attacks via Timbre Watermarking](https://www.ndss-symposium.org/wp-content/uploads/2024-200-paper.pdf) · [arXiv](https://arxiv.org/abs/2312.03410) | NDSS | [Code](https://github.com/TimbreWatermarking/TimbreWatermarking) | TimbreWatermarking: marks reference speech so a timbre-linked signal persists through voice cloning. |
| 2024 | [ProMark: Proactive Diffusion Watermarking for Causal Attribution](https://openaccess.thecvf.com/content/CVPR2024/papers/Asnani_ProMark_Proactive_Diffusion_Watermarking_for_Causal_Attribution_CVPR_2024_paper.pdf) · [arXiv](https://arxiv.org/abs/2403.09914) | CVPR | — | Proactive training-data watermarks for attribution to learned concepts and styles. |
| 2024 | [Watermarking Makes Language Models Radioactive](https://arxiv.org/abs/2402.14904) | NeurIPS | [Code](https://github.com/facebookresearch/radioactive-watermark) | Detects training on outputs of a watermarked language model. Repository archived; code remains available. |
| 2024 | [Watermarking Training Data of Music Generation Models](https://arxiv.org/abs/2412.08549) | Preprint | — | Studies watermark inheritance in music generation and the effect of audio tokenization on watermark survival. |
| 2023 | [Generative Watermarking Against Unauthorized Subject-Driven Image Synthesis](https://arxiv.org/abs/2306.07754) | Preprint | — | Protects reference images by making unauthorized subject-driven synthesis reproduce a watermark. |
| 2023 | [Hey That's Mine! Imperceptible Watermarks are Preserved in Diffusion Generated Outputs](https://arxiv.org/abs/2308.11123) | Preprint | — | Examines whether imperceptible training-image watermarks persist in diffusion-generated outputs. |
| 2021 | [Artificial Fingerprinting for Generative Models: Rooting Deepfake Attribution in Training Data](https://ieeexplore.ieee.org/document/9711167) · [arXiv](https://arxiv.org/abs/2007.08457) | ICCV | [Code](https://github.com/ningyu1991/ArtificialGANFingerprints) | Embeds fingerprints in training images so trained GANs reproduce them. |


<a id="model-ip"></a>
### Model ownership and trigger-based verification

Ownership checks can require special inputs or model access and may not apply to arbitrary generated content.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2025 | [RoMa: A Robust Model Watermarking Scheme for Protecting IP in Diffusion Models](https://proceedings.neurips.cc/paper_files/paper/2025/hash/2f4837a74fe73997ad9bfd3b655f0f3d-Abstract-Conference.html) | NeurIPS | [Code](https://github.com/xiekks/RoMa) | Diffusion-model ownership verification and resistance to model modification. |
| 2025 | [SleeperMark: Towards Robust Watermark against Fine-Tuning Text-to-image Diffusion Models](https://openaccess.thecvf.com/content/CVPR2025/html/Wang_SleeperMark_Towards_Robust_Watermark_against_Fine-Tuning_Text-to-image_Diffusion_Models_CVPR_2025_paper.html) | CVPR | [Code](https://github.com/taco-group/SleeperMark) | Trigger-based model ownership watermark designed to persist through downstream fine-tuning. |
| 2024 | [Intellectual Property Protection of Diffusion Models via the Watermark Diffusion Process](https://link.springer.com/chapter/10.1007/978-981-96-0567-5_21) · [arXiv](https://arxiv.org/abs/2306.03436) | WISE | [Code](https://github.com/senp98/wdm) | Jointly learns a watermark diffusion process and the normal generation process for model ownership verification. |
| 2023 | [A Recipe for Watermarking Diffusion Models](https://arxiv.org/abs/2303.10137) | Preprint | [Code](https://github.com/yunqing-me/WatermarkDM) | Studies training-data watermark transfer and trigger-based ownership verification for diffusion models. |
| 2023 | [Watermarking Diffusion Model](https://arxiv.org/abs/2305.12502) | Preprint | — | NAIVEWM and FIXEDWM use special prompts to trigger ownership-verification outputs. |


<a id="attacks"></a>
## Attacks and robustness analysis

Removal, forgery, detector evasion, key inference, model purification, and transformations that weaken watermark recovery. Attack results are specific to the tested schemes and access assumptions.


<a id="attack-image"></a>
### Image and diffusion watermark attacks

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [Cryptanalysis of LDPC-Based Pseudorandom Error-Correcting Codes](https://www.usenix.org/conference/usenixsecurity26/presentation/wang-tianrui) | USENIX Security | [Code](https://github.com/CryptoAILab/AttackPRC) | Cryptanalysis of specific LDPC-based PRC constructions; relevant to PRC image watermark assumptions. |
| 2026 | [Rethinking Forgery Attacks on Semantic Watermarks in Black-Box Settings: A Geometric Distortion Perspective](https://proceedings.mlr.press/v306/lee26bm.html) | ICML | — | Examines geometry-based forgery of semantic diffusion watermarks. |
| 2025 | [A Transfer Attack to Image Watermarks](https://arxiv.org/abs/2403.15365) | ICLR | [Code](https://github.com/hifi-hyp/Watermark-Transfer-Attack) | Uses surrogate models to transfer watermark-removal perturbations without target detector access. |
| 2025 | [Black-Box Forgery Attacks on Semantic Watermarks for Diffusion Models](https://openaccess.thecvf.com/content/CVPR2025/papers/Muller_Black-Box_Forgery_Attacks_on_Semantic_Watermarks_for_Diffusion_Models_CVPR_2025_paper.pdf) | CVPR | [Code](https://github.com/and-mill/semantic-forgery) | Studies forged provenance under black-box access to semantic watermark detectors. |
| 2025 | [Forging and Removing Latent-Noise Diffusion Watermarks Using a Single Image](https://arxiv.org/abs/2504.20111) | Preprint | [Code](https://github.com/anubhav1997/watermark_forgery_removal) | Single-image attacks against latent-noise diffusion watermarking. |
| 2025 | [Transferable Black-Box One-Shot Forging of Watermarks via Image Preference Models](https://proceedings.neurips.cc/paper_files/paper/2025/file/47fee9cd8a252161dec7cb48ec0ca2f2-Paper-Conference.pdf) | NeurIPS | [Code](https://github.com/facebookresearch/videoseal/tree/main/wmforger) | WmForger: transferable watermark forgery using image preference models. |
| 2025 | [UnMarker: A Universal Attack on Defensive Image Watermarking](https://arxiv.org/abs/2405.08363) | IEEE S&P | [Code](https://github.com/andrekassis/ai-watermark) | Removal attack evaluated across multiple image watermarking families. |
| 2024 | [Invisible Image Watermarks Are Provably Removable Using Generative AI](https://arxiv.org/abs/2306.01953) | NeurIPS | [Code](https://github.com/XuandongZhao/WatermarkAttacker) · [Earlier workshop version](https://openreview.net/forum?id=9xbArVQhnQ) | Regeneration attacks and theory of invisible watermark removal; earlier version: Generative Autoencoders as Watermark Attackers (ICML 2023 Deployable Generative AI workshop). |
| 2024 | [Leveraging Optimization for Adaptive Attacks on Image Watermarks](https://proceedings.iclr.cc/paper_files/paper/2024/hash/33ae99f57181554090fb6a9071a512a9-Abstract-Conference.html) · [arXiv](https://arxiv.org/abs/2309.16952v2) | ICLR | [Code](https://github.com/spotai-mbzuai/adaptive-watermark-attacks) | Adaptive image-watermark attacks using differentiable surrogate keys. |
| 2024 | [Robustness of AI-Image Detectors: Fundamental Limits and Practical Attacks](https://openreview.net/forum?id=dLoAdIKENc) · [arXiv](https://arxiv.org/abs/2310.00076) | ICLR | [Code](https://github.com/mehrdadsaberi/watermark_robustness) | Studies detector limits and regeneration/model-substitution attacks on image watermarks. |
| 2024 | [Stable Signature is Unstable: Removing Image Watermark from Diffusion Models](https://arxiv.org/abs/2405.07145) | Preprint | — | Fine-tuning attack that removes Stable Signature from a released decoder. |
| 2024 | [Steganalysis on Digital Watermarking: Is Your Defense Truly Impervious?](https://arxiv.org/abs/2406.09026) | Preprint | [Code](https://github.com/showlab/watermark-steganalysis) | Studies extraction, removal, and forgery of content-agnostic watermarks through steganalysis; repository releases the core benchmark. |
| 2023 | [Evading Watermark based Detection of AI-Generated Content](https://dl.acm.org/doi/10.1145/3576915.3623189) · [arXiv](https://arxiv.org/abs/2305.03807) | ACM CCS | [Code](https://github.com/zhengyuan-jiang/WEvade) | WEvade: adversarial evasion of watermark-based AI-image detection. |
| 2023 | [Warfare: Breaking the Watermark Protection of AI-Generated Content](https://arxiv.org/abs/2310.07726) | Preprint | [Code](https://github.com/GuanlinLee/warfare) | Warfare: unified watermark removal and forgery using generative models; earlier title: Towards the Vulnerability of Watermarking Artificial Intelligence Generated Content. |
| 2020 | [On Attribution of Deepfakes](https://arxiv.org/abs/2008.09194) | Preprint | — | Studies attribution and deniability, including images falsely attributed to another generator. |


<a id="attack-text"></a>
### Text watermark attacks and cross-language robustness

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [Beyond A Fixed Seal: Adaptive Stealing Watermark in Large Language Models](https://aclanthology.org/2026.findings-acl.1036/) | Findings of ACL | [Code](https://github.com/DrankXs/AdaptiveStealingWatermark) | Adaptive watermark stealing and query efficiency. |
| 2025 | [Is Multilingual LLM Watermarking Truly Multilingual? A Simple Back-Translation Solution](https://arxiv.org/abs/2510.18019) | Preprint | [Code](https://github.com/asimzz/steam) | STEAM: back-translation-based detection for multilingual watermark robustness. |
| 2025 | [Revealing Weaknesses in Text Watermarking Through Self-Information Rewrite Attacks](https://proceedings.mlr.press/v267/cheng25c.html) | ICML | [Code](https://github.com/Allencheng97/Self-information-Rewrite-Attack) | SIRA: targeted rewriting based on token self-information. |
| 2025 | [Sandcastles in the Storm: Revisiting the (Im)possibility of Strong Watermarking](https://aclanthology.org/2025.acl-long.1436/) | ACL | — | Re-examines quality-preserving removal attacks and their practical limits. |
| 2025 | [Watermark Smoothing Attacks against Language Models](https://aclanthology.org/2025.findings-emnlp.264/) | Findings of EMNLP | [Code](https://github.com/changhongyan123/watermark_smoothing) | Watermark removal through smoothing attacks. |
| 2024 | [Can Watermarks Survive Translation? On the Cross-lingual Consistency of Text Watermark for Large Language Models](https://arxiv.org/abs/2402.14007) | ACL | — | Evaluates watermark survival under translation and cross-language transformations. |
| 2024 | [Watermark Stealing in Large Language Models](https://proceedings.mlr.press/v235/jovanovic24a.html) | ICML | [Code](https://github.com/eth-sri/watermark-stealing) | Learns watermark rules from model queries to support removal and spoofing. |
| 2023 | [Can AI-Generated Text be Reliably Detected?](https://arxiv.org/abs/2303.11156) | Preprint | [Code](https://github.com/vinusankars/Reliability-of-AI-text-detectors) | Paraphrasing attacks and detection limitations; covers watermarked and unwatermarked text. |


<a id="attack-audio"></a>
### Audio watermark robustness

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [The Vulnerability of Neural Audio Watermarks under Speech Enhancement](https://arxiv.org/abs/2609.29040) | Preprint | — | Evaluates removal using speech enhancement and added noise against multiple audio watermark systems. |


<a id="bench"></a>
## Benchmarks, detection, and evaluation

Includes experimental frameworks, comparative studies, watermark localization, and detection without the original decoder. These papers should not all be read as new embedding algorithms.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [MarkDiffusion: An Open-Source Toolkit for Generative Watermarking of Latent Diffusion Models](https://arxiv.org/abs/2509.10569) | JMLR (accepted; author-reported) | [Code](https://github.com/THU-BPM/MarkDiffusion) | Diffusion-watermark implementations and evaluation. Author repository reports JMLR acceptance. |
| 2026 | [Watermarking Degrades Alignment in Language Models: Analysis and Mitigation](https://arxiv.org/abs/2506.04462) | TMLR (accepted; author-reported) | [Code](https://github.com/dapurv5/alignmark) | Studies model-specific effects on safety, truthfulness, and helpfulness; proposes reward-based Alignment Resampling. |
| 2026 | [WMVLM: Evaluating Diffusion Model Image Watermarking via Vision-Language Models](https://proceedings.mlr.press/v306/yang26bs.html) | ICML | — | Vision-language-model-based evaluation of diffusion watermarks. |
| 2025 | [Mark My Words: Analyzing and Evaluating Language Model Watermarks](https://arxiv.org/abs/2312.00273) | IEEE SaTML | [Code](https://github.com/wagner-group/MarkMyWords) | Text-watermark benchmark covering quality, detection length, and tamper resistance. |
| 2025 | [On the Coexistence and Ensembling of Watermarks](https://proceedings.nips.cc/paper_files/paper/2025/hash/99fecf765ecf62c3e3175ef2278f3315-Abstract-Conference.html) | NeurIPS | — | Studies the interaction of multiple image watermarks and ensembles. |
| 2025 | [WaterSeeker: Pioneering Efficient Detection of Watermarked Segments in Large Documents](https://aclanthology.org/2025.findings-naacl.156/) | Findings of NAACL | [Code](https://github.com/THU-BPM/WaterSeeker) | Localizes short watermarked segments inside long documents. |
| 2024 | [AudioMarkBench: Benchmarking Robustness of Audio Watermarking](https://arxiv.org/abs/2406.06979) | NeurIPS Datasets and Benchmarks | [Code](https://github.com/mileskuo42/AudioMarkBench) | Audio watermark evaluation under common and adversarial transformations. |
| 2024 | [Finding needles in a haystack: A Black-Box Approach to Invisible Watermark Detection](https://www.ecva.net/papers/eccv_2024/papers_ECCV/papers/04872.pdf) · [arXiv](https://arxiv.org/abs/2403.15955) | ECCV | [Code](https://github.com/pmzzs/WMD) | WMD: detects invisible watermarks using reference datasets without the original watermark decoder. |
| 2024 | [MarkLLM: An Open-Source Toolkit for LLM Watermarking](https://aclanthology.org/2024.emnlp-demo.7/) · [arXiv](https://arxiv.org/abs/2405.10051) | EMNLP Demonstrations | [Code](https://github.com/THU-BPM/MarkLLM) | Implementations, evaluation tools, and visualizations for text watermarking. |
| 2024 | [WaterBench: Towards Holistic Evaluation of Watermarks for Large Language Models](https://aclanthology.org/2024.acl-long.83/) | ACL | [Code](https://github.com/THU-KEG/WaterBench) | Joint generation-quality and detection evaluation under comparable watermark strengths. |
| 2024 | [WAVES: Benchmarking the Robustness of Image Watermarks](https://proceedings.mlr.press/v235/an24a.html) | ICML | [Code](https://github.com/umd-huang-lab/WAVES) | WAVES: standardized robustness evaluation covering distortions, regeneration, and adversarial attacks. |


<a id="theory"></a>
## Theory and security foundations

Formal results must be interpreted under their stated access models, distributional assumptions, quality oracles, and computational assumptions.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2025 | [A Statistical Framework of Watermarks for Large Language Models: Pivot, Detection Efficiency and Optimal Rules](https://doi.org/10.1214/24-AOS2468) | The Annals of Statistics | [Code](https://github.com/lx10077/WatermarkFramework) | Formal framework for watermark detection efficiency and statistical test design. |
| 2025 | [Watermarking Language Models for Many Adaptive Users](https://www.computer.org/csdl/proceedings-article/sp/2025/223600a084/21B7RqlGjde) | IEEE S&P | — | Formal watermarking security in an adaptive multi-user setting. |
| 2024 | [Pseudorandom Error-Correcting Codes](https://arxiv.org/abs/2402.09370) | CRYPTO | — | PRCs: foundational coding primitive used in cryptographic generative-model watermarks. |
| 2024 | [Undetectable Watermarks for Language Models](https://proceedings.mlr.press/v247/christ24a.html) | COLT | — | Formal constructions and computational undetectability under cryptographic assumptions. |
| 2024 | [Watermarks in the Sand: Impossibility of Strong Watermarking for Language Models](https://proceedings.mlr.press/v235/zhang24o.html) · [arXiv](https://arxiv.org/abs/2311.04378) | ICML | [Code](https://github.com/hlzhang109/impossibility-watermark) | Quality-preserving random-walk removal and limits of strong watermark robustness under stated assumptions. |
| 2006 | [Watermarking is not Cryptography](https://link.springer.com/chapter/10.1007/11922841_1#citeas) · [PDF](http://www0.cs.ucl.ac.uk/staff/ingemar/Content/papers/2006/IWDW2006.pdf) | IWDW | — | Distinguishes watermark robustness and security from cryptographic guarantees. |


<a id="survey"></a>
## Surveys

Starting points for broader reading and terminology; see [related surveys](#survey-related) for passive detection and wider AI security.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [A Survey on LLM Watermarking: Theory and Deployment](https://arxiv.org/abs/2607.10103) | Preprint | — | Survey connecting formal watermark properties with deployment considerations. |
| 2026 | [Watermarking techniques for large language models: a survey](https://link.springer.com/article/10.1007/s10462-025-11474-6) | Artificial Intelligence Review | — | Reviews language-model watermarking and its relationship to traditional watermarking. |
| 2025 | [Secure and Robust Watermarking for AI-generated Images: A Comprehensive Survey](https://arxiv.org/abs/2510.02384) | Preprint | — | Survey of image-generation watermarking, threat models, and robustness. |
| 2025 | [SoK: Watermarking for AI-Generated Content](https://ieeexplore.ieee.org/document/11023391/) · [arXiv](https://arxiv.org/abs/2411.18479) | IEEE S&P | — | Systematizes definitions, goals, threat models, evaluation, and open problems across generative-content watermarking. |
| 2025 | [Watermarking for AI Content Detection: A Review on Text, Visual, and Audio Modalities](https://arxiv.org/abs/2504.03765) | ICLR Workshop on GenAI Watermarking (WMARK) | — | Cross-modal survey organized around effectiveness, robustness, and practical use. |
| 2024 | [A Survey of Text Watermarking in the Era of Large Language Models](https://dl.acm.org/doi/10.1145/3691626) | ACM Computing Surveys | — | Survey of text-watermark methods, evaluation, and attacks. |
| 2024 | [Copyright Protection in Generative AI: A Technical Perspective](https://arxiv.org/abs/2402.02333) | Preprint | — | Technical overview of copyright protection for generative AI, including watermarking. |
| 2022 | [A Comprehensive Survey on Robust Image Watermarking](https://www.sciencedirect.com/science/article/pii/S0925231222002533) | Neurocomputing | — | Survey of robust image-watermarking methods and evaluation. |
| 2021 | [A Comprehensive Review on Digital Image Watermarking](https://ceur-ws.org/Vol-2889/PAPER_14.pdf) · [arXiv](https://arxiv.org/abs/2207.06909) | Workshop on Computer Networks and Communications (CEUR-WS) | — | Broad review of digital image watermarking; distinct from the Neurocomputing survey above. |
| 2021 | [A Systematic Review on Model Watermarking for Neural Networks](https://www.frontiersin.org/articles/10.3389/fdata.2021.729663/full) · [arXiv](https://arxiv.org/abs/2009.12153) | Frontiers in Big Data | — | Survey of neural-network model watermarking; distinguish model ownership from output provenance. |


<a id="software"></a>
## Software and provenance standards

Research toolkits with accompanying papers appear in [benchmarks and evaluation](#bench). The resources below are software or standards; their inclusion does not imply a separate peer-reviewed paper.

| Resource | Scope | Notes |
| --- | --- | --- |
| [OmniSeal Bench](https://github.com/facebookresearch/omnisealbench) | Image, video, and audio evaluation | Multimodal watermark benchmark software. Repository archived in June 2026. |
| [Content Seal](https://facebookresearch.github.io/content-seal/) | Cross-domain research collection | Author-maintained entry point for watermark models, papers, and code. |
| [Perth](https://github.com/resemble-ai/perth) | Audio | Open audio watermarking tool. |
| [invisible-watermark](https://github.com/ShieldMnt/invisible-watermark) | Image baselines | Python implementations including conventional transforms and RivaGAN. |
| [SynthID](https://deepmind.google/models/synthid/) | Text, image, audio, and video | Official product/research overview; the [text reference implementation](https://github.com/google-deepmind/synthid-text) is public. Availability differs by modality. |
| [C2PA Content Credentials](https://spec.c2pa.org/specifications/specifications/2.4/specs/ContentCredentials.html) | Signed provenance | Provenance manifests are distinct from embedded watermarks. Soft bindings can use invisible watermarks or content fingerprints to reconnect an asset with its credentials. |

<a id="related"></a>
## Related work and archival entries

### Adjacent research

These papers provide relevant context but are not all generative-output watermarking methods. In particular, anti-training perturbations, perceptual hashing, and model extraction should not be confused with recoverable content watermarks.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2026 | [Missing the Mark: Adoption of Watermarking for Generative AI Systems in Practice and Implications Under the New EU AI Act](https://onlinelibrary.wiley.com/doi/10.1002/poi3.70041) | Policy & Internet | — | Empirical study of marking adoption and deployment; distinguishes technical methods from policy obligations. |
| 2023 | [Adversarial Attack for Robust Watermark Protection Against Inpainting-based and Blind Watermark Removers](https://dl.acm.org/doi/10.1145/3581783.3612034) | ACM MM | — | Protects visible watermarks against inpainting and blind removal; adjacent to invisible provenance watermarking. |
| 2023 | [DUAW: Data-free Universal Adversarial Watermark against Stable Diffusion Customization](https://arxiv.org/abs/2308.09889) | Preprint | — | Universal adversarial perturbations against Stable Diffusion customization; adjacent anti-training defense. |
| 2023 | [Glaze: Protecting artists from style mimicry by text-to-image models](https://www.usenix.org/conference/usenixsecurity23/presentation/shan) · [arXiv](https://arxiv.org/abs/2302.04222) | USENIX Security | [Project](https://glaze.cs.uchicago.edu/) | Anti-style-mimicry perturbations; data protection rather than recoverable provenance watermarking. |
| 2023 | [Squint Hard Enough: Attacking Perceptual Hashing with Adversarial Machine Learning](https://www.usenix.org/conference/usenixsecurity23/presentation/prokos) | USENIX Security | — | Adversarial attacks on perceptual hashing; relevant to content binding, not an embedded watermark scheme. |
| 2022 | [ChartStamp: Robust Chart Embedding for Real-World Applications](https://dl.acm.org/doi/pdf/10.1145/3503161.3548286) | ACM MM | [Code](https://github.com/CGCL-codes/ChartStamp) | Chart-specific watermark embedding for real-world capture; adjacent digital-media application. |
| 2022 | [Diffusion Models for Adversarial Purification](https://proceedings.mlr.press/v162/nie22a.html) · [arXiv](https://arxiv.org/abs/2205.07460) | ICML | [Code](https://github.com/NVlabs/DiffPure) | DiffPure: adversarial purification method commonly reused as a watermark-removal baseline. |
| 2022 | [Model Extraction Attack and Defense on Deep Generative Models](https://iopscience.iop.org/article/10.1088/1742-6596/2189/1/012024/meta) | Journal of Physics: Conference Series | — | Studies extraction and defenses for deep generative models. |
| 2021 | [Stealing Machine Learning Models: Attacks and Countermeasures for Generative Adversarial Networks](https://dl.acm.org/doi/10.1145/3485832.3485838) · [arXiv](https://arxiv.org/abs/2101.02069) | ACSAC | — | Studies GAN model extraction and watermark-based countermeasures. |


<a id="survey-related"></a>
### Related surveys

Broader security and passive deepfake-detection surveys retained as background.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2024 | [Detecting Multimedia Generated by Large AI Models: A Survey](https://arxiv.org/abs/2402.00045) | Preprint | [Reading list](https://github.com/Purdue-M2/Detect-LAIM-generated-Multimedia-Survey) | Survey covering passive detection and active watermark-based detection of generated multimedia. |
| 2023 | [Audio Deepfake Detection: A Survey](https://arxiv.org/abs/2308.14970) | Preprint | — | Audio deepfake detection survey; useful surrounding context rather than a watermark-method survey. |
| 2023 | [Security and Privacy on Generative Data in AIGC: A Survey](https://arxiv.org/abs/2309.09435) | Preprint | — | Broader survey of security and privacy for generated data. |


### Withdrawn manuscripts

Retained for bibliographic transparency; do not treat these as active or accepted contributions.

| Year | Paper | Venue / status | Code / resources | Focus |
| --- | --- | --- | --- | --- |
| 2024 | [PiGW: A Plug-in Generative Watermarking Framework](https://arxiv.org/abs/2403.12053) | Withdrawn preprint | — | Withdrawn arXiv manuscript; retained for bibliographic transparency. |


<a id="maintenance"></a>
## Maintenance and contribution notes

For additions or corrections, provide the exact paper title, a stable paper link, the verified publication year and venue/track, and an author-linked repository or project page. State whether code is released, partial, a placeholder, or an independent implementation. Include a short description of the modality, insertion point, and purpose. Prefer proceedings or publisher records for acceptance; a submission page or a citation in another paper is not sufficient evidence of acceptance. Do not add a second row for a renamed preprint or later publication of the same paper.

<details>
<summary>Venue abbreviations</summary>

| Abbreviation | Venue |
| --- | --- |
| AAAI | AAAI Conference on Artificial Intelligence |
| ACM CCS | ACM Conference on Computer and Communications Security |
| ACM MM / MM-Asia | ACM International Conference on Multimedia / ACM Multimedia Asia |
| ACL | Annual Meeting of the Association for Computational Linguistics |
| ACSAC | Annual Computer Security Applications Conference |
| COLT | Conference on Learning Theory |
| CRYPTO | International Cryptology Conference |
| CVPR / CVPRW | IEEE/CVF Conference on Computer Vision and Pattern Recognition / its workshops |
| EACL | Conference of the European Chapter of the Association for Computational Linguistics |
| ECCV | European Conference on Computer Vision |
| EMNLP | Conference on Empirical Methods in Natural Language Processing |
| ICASSP | IEEE International Conference on Acoustics, Speech and Signal Processing |
| ICCV | IEEE/CVF International Conference on Computer Vision |
| ICLR | International Conference on Learning Representations |
| ICML | International Conference on Machine Learning |
| IEEE S&P | IEEE Symposium on Security and Privacy |
| IEEE SaTML | IEEE Conference on Secure and Trustworthy Machine Learning |
| IEEE TMM / TSE | IEEE Transactions on Multimedia / Software Engineering |
| IJCAI | International Joint Conference on Artificial Intelligence |
| IWDW | International Workshop on Digital Watermarking |
| JMLR / TMLR | Journal of Machine Learning Research / Transactions on Machine Learning Research |
| NAACL | Conference of the NAACL chapter of the Association for Computational Linguistics |
| NDSS | Network and Distributed System Security Symposium |
| NeurIPS | Conference on Neural Information Processing Systems |
| TAISAP | ACM Transactions on AI Security and Privacy |
| WISE | International Conference on Web Information Systems Engineering |

</details>

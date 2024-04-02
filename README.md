# Awesome-GenAI-Watermarking

This repo include papers about the watermarking methods for generative AI models. For now, it focuses on the visual
domain.

# Papers on Watermarking Diffusion Models for Generated Asset Detection and User Identification

| Paper                                                                                                                                                                                                                        | Venue                                | Venue Year / Last Updated | Code                                                             | Alternative PDF Source                                   | Notes                                                                                                                                                                                                                                                                                                                                                                                            |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|---------------------------|------------------------------------------------------------------|----------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [The Stable Signature: Rooting Watermarks in Latent Diffusion Models](https://openaccess.thecvf.com/content/ICCV2023/html/Fernandez_The_Stable_Signature_Rooting_Watermarks_in_Latent_Diffusion_Models_ICCV_2023_paper.html) | ICCV                                 | 2023                      | [code](https://github.com/facebookresearch/stable_signature.git) | [Arxiv](https://arxiv.org/abs/2303.15435)                | - Finetune a model to reveal a secret message in its output.<br>- robust to watermark removal and model purification (quality deterioration)<br>- Static watermarking                                                                                                                                                                                                                            |
| [Flexible and Secure Watermarking for Latent Diffusion Model](https://dl.acm.org/doi/10.1145/3581783.3612448)                                                                                                                | MM                                   | 2023                      | -                                                                | -                                                        | - References Stable Signature and improves by adding flexibility by allowing for embedding different messages w.o. finetuning                                                                                                                                                                                                                                                                    |
| [RoSteALS: Robust Steganography using Autoencoder Latent Space](https://ieeexplore.ieee.org/document/10208817)                                                                                                               | CVPR Workshops (CVPRW)               | 2023                      | [code](https://github.com/coriverchen/Robust_Steganography.git)  | [Arxiv](https://arxiv.org/abs/2304.03400)                | -                                                                                                                                                                                                                                                                                                                                                                                                |
| [DiffusionShield: A Watermark for Copyright Protection against Generative Diffusion Models](https://neurips.cc/virtual/2023/74895)                                                                                           | NeurIPS Workshop on Diffusion Models | 2023                      | -                                                                | [Arxiv](https://arxiv.org/abs/2306.04642)                | -                                                                                                                                                                                                                                                                                                                                                                                                |
| A Recipe for Watermarking Diffusion Models                                                                                                                                                                                   | -                                    | 2024                      | [code](https://github.com/yunqing-me/WatermarkDM.git)            | [Arxiv](https://arxiv.org/abs/2303.10137)                | - Framework for 1. small unconditional/class-conditional DMs via training from scratch on watermarked data and 2. text-to-image DMs via finetuning a backdoor-trigger-output<br>- Lots of references on watermarking discriminative models<br>- Static watermarking                                                                                                                              |
| Tree-Ring Watermarks: Fingerprints for Diffusion Images that are Invisible and Robust                                                                                                                                        | -                                    | 2023                      | -                                                                | [Arxiv](https://arxiv.org/abs/2305.20030)                | - Embedding tree-ring into FFT of Noise Vector.<br>- TODO: Is this just a coarse variant of embedding a message as in stable signature?<br>- TODO: Very robust against even malicious attacks at time of upload?<br>- Says: "watermark currently deployed in Stable Diffusion is [Secure spread spectrum watermarking for images, audio and video](http://ieeexplore.ieee.org/document/560429/)" |
| Intellectual Property Protection of Diffusion Models via the Watermark Diffusion Process                                                                                                                                     | -                                    | 2023                      | -                                                                | [Arxiv](https://arxiv.org/abs/2306.03436)                | - Threat model: Check ownership of model by having access to the model<br>- Hard to read<br>- Explains difference between **static and dynamic watermarking** with many references                                                                                                                                                                                                               |
| Securing Deep Generative Models with Universal Adversarial Signature                                                                                                                                                         | -                                    | 2023                      | [code](https://github.com/zengxianyu/genwm)                      | [Arxiv](https://arxiv.org/abs/2305.16310)                | - Step 1: find optimal signature for an image individually. Step 2: Finetune a GenAI model on these images.                                                                                                                                                                                                                                                                                      |
| Watermarking Diffusion Model                                                                                                                                                                                                 | -                                    | 2023                      | -                                                                | [Arxiv](https://arxiv.org/abs/2305.12502)                | - Finetuning a backdoor-trigger-output<br>- Static watermarking<br>- CISPA authors                                                                                                                                                                                                                                                                                                               |
| Catch You Everything Everywhere: Guarding Textual Inversion via Concept Watermarking                                                                                                                                         | -                                    | 2023                      | -                                                                | [Arxiv](https://arxiv.org/abs/2309.05940)                | - Guards concepts obtained through textual inversion ([An Image is Worth One Word: Personalizing Text-to-Image Generation using Textual Inversion](https://arxiv.org/abs/2208.01618)) from abuse by allowing to identify concepts in generated images.<br>- Very interesting references on company and government stances on watermarking                                                        |
| Generative Watermarking Against Unauthorized Subject-Driven Image Synthesis                                                                                                                                                  | -                                    | 2023                      | -                                                                | [Arxiv](https://arxiv.org/abs/2306.07754)                | - Different from Glaze in that style synthesis from protected source images is not prevented, but recognizable via watermarks<br>- CISPA authors                                                                                                                                                                                                                                                 |
| Towards the Vulnerability of Watermarking Artificial Intelligence Generated Content                                                                                                                                          | -                                    | 2024                      | -                                                                | [OpenReview](https://openreview.net/forum?id=xY4861TVUc) | - Watermark removal and forgery in one method, using GAN<br>- References two types of watermarking: **1. Learn/finetune model to produce watermarked output and 2. post-hoc watermarking after the fact** (static vs. dynamic, see "Intellectual Property Protection of Diffusion Models via the Watermark Diffusion Process")                                                                   |
| [Robustness of AI-Image Detectors: Fundamental Limits and Practical Attacks](https://openreview.net/forum?id=dLoAdIKENc&referrer=%5Bthe%20profile%20of%20Soheil%20Feizi%5D(%2Fprofile%3Fid%3D~Soheil_Feizi2))                | ICLR (Poster)                        | 2024                      | [code](https://github.com/mehrdadsaberi/watermark_robustness)    | [Arxiv](https://arxiv.org/abs/2310.00076)                | - Watermark removal and forgery, analysis on the tradeoff between watermark fidelity and robustness                                                                                                                                                                                                                                                                                              |
| A Transfer Attack to Image Watermarks                                                                                                                                                                                        | -                                    | 2024                      | -                                                                | [Arxiv](https://arxiv.org/abs/2403.15365)                | - Watermark removal by "no-box"-attack on detectors (no access to detector-API, instead training classifier to distinguish watermarked and vanilla images)                                                                                                                                                                                                                                       |

## Differences Between Watermarking Schemes
- Static vs. Dynamic Watermarking
  - Static watermarking: "[...] specific pattern in its static content, such as a particular distribution of parameters" (see "Intellectual Property Protection of Diffusion Models via the Watermark Diffusion Process") -> TODO
  - Dynamic Watermarking: "[...] specific pattern in model’s dynamic contents, such as its behavior.", e.g. trigger-prompt-watermark-backdoors
- Model Watermarking vs. Post-Hoc Watermarking
  - Model Watermarking: The model is manipulated in a way that its usual generating process produces watermarked output - This DOES protect open models
  - Post-Hoc Watermarking: The Watermark is added after the fact, in a separate process. -> This does NOT protect open models, as the watermark embedding procedure can be simply disabled
- Mechanism of training the watermarking behaviour into a model
  - Via Watermarked training data (e.g. [Artificial Fingerprinting for Generative Models: Rooting Deepfake Attribution in Training Data](https://ieeexplore.ieee.org/document/9711167)) -> "Plug-and-play", architecture-agnostic solution
  - Joint fine-tuning of model and a decoder (taken from a encoder/decoder-pair) on few samples ([The Stable Signature: Rooting Watermarks in Latent Diffusion Models](https://openaccess.thecvf.com/content/ICCV2023/html/Fernandez_The_Stable_Signature_Rooting_Watermarks_in_Latent_Diffusion_Models_ICCV_2023_paper.html)) -> Requires latent generative model

# Related News

- [Coalition for Content Provenance and Authenticity (C2PA)](https://c2pa.org/)
- Is based on a trust model with signing authorities which certify signer of some digital asset through a chain of trust, similar to web PKI. 
    - [C2PA Specifications](https://c2pa.org/specifications/specifications/1.3/index.html)
        - [Explainer](https://c2pa.org/specifications/specifications/1.2/explainer/Explainer.html)
            - "Provenance generally refers to the facts about the history of a piece of digital content assets (image,
              video, audio recording, document). C2PA enables the authors of provenance data to securely bind statements
              of provenance data to instances of content using their unique credentials. These provenance statements are
              called assertions by the C2PA. They may include assertions about who created the content and how, when,
              and where it was created. They may also include assertions about when and how it was edited throughout its
              life. The content author, and publisher (if authoring provenance data) always has control over whether to
              include provenance data as well as what assertions are included, such as whether to include identifying
              information (in order to allow for anonymous or pseudonymous assets). Included assertions can be removed
              in later edits without invalidating or removing all of the included provenance data in a process called
              redaction."
            - "In the C2PA Specifications, trust decisions are made by the consumer of the asset based on the identity
              of the actor(s) who signed the provenance data along with the information in the assertions contained in
              the provenance. This signing takes place at each significant moment in an asset’s life (e.g., creation,
              editing, etc.) through the use of the actor’s unique credentials and ensures that the provenance data
              remains cryptographically bound to the newly created or updated asset."
            - -> Less about Watermarking, and more about allowing authors and subsequent actors to sign assets and make
              this act publicly known to establish the asset's history.
- [Open-source tools for content authenticity and provenance](https://opensource.contentauthenticity.org/)
  - Uses manifests as defined in [C2PA Specifications](https://c2pa.org/specifications/specifications/1.3/index.html)
  - Enables camera manufacturers to insert authenticity meta-data on-device 
- [Deepmind SynthID](https://deepmind.google/technologies/synthid/)
    - Images ([Vertex AI imagen](https://cloud.google.com/vertex-ai/generative-ai/docs/image/overview))
    - Audio ([Google DeepMind's Lyria](https://deepmind.google/discover/blog/transforming-the-future-of-music-creation/))
        - Transfer artist's styles to audio prompts (style transfer)
        - Most likely, the watermark will include the prompt used to generate audio (e.g. Artist name). This allows for
          copyright claims.
    - In Beta (as of 26 Mar 2024)
    - Closed off as can be (as of 26 Mar 2024)
- Google hosted a workshop in June 2023 ([Identifying and Mitigating the Security Risks of Generative AI](https://arxiv.org/abs/2308.14840)): "Watermarking was mentioned as a promising mitigation. They are robust when attacker has no access to detection algorithm"
- Watermarking is identified as tool for establishing trust in a post GenAI environment ([OpenAI moving AI governance forward statement](https://openai.com/blog/moving-ai-governance-forward), [Google "Our commitment to advancing bold and responsible AI, together"](https://blog.google/outreach-initiatives/public-policy/our-commitment-to-advancing-bold-and-responsible-ai-together/), [Biden-⁠Harris Administration Secures Voluntary Commitments from Leading Artificial Intelligence Companies to Manage the Risks Posed by AI ](https://www.whitehouse.gov/briefing-room/statements-releases/2023/07/21/fact-sheet-biden-harris-administration-secures-voluntary-commitments-from-leading-artificial-intelligence-companies-to-manage-the-risks-posed-by-ai/))
- Watermarks can be easily inserted into Stable Diffusion models [invisible-watermark repo](https://github.com/ShieldMnt/invisible-watermark) and referenced by the official [Stable Diffusion repo](https://github.com/CompVis/stable-diffusion?tab=readme-ov-file)

# Misc Papers (to be categorized...)

| Paper                                                                                                                                                                | Venue         | Venue Year / Last Updated | Code                                                          | Alternative PDF Source                        | Notes                                                                                                                                                                                             |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------------------|---------------------------------------------------------------|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [HiDDeN: Hiding Data With Deep Networks](https://dl.acm.org/doi/abs/10.1007/978-3-030-01267-0_40)                                                                    | ECCV          | 2018                      | [code](https://github.com/jirenz/HiDDeN)                      | [Arxiv](https://arxiv.org/pdf/1807.09937.pdf) | - Main tool used in Stable Signature<br>- Contains differentiable approx. of JPEG compression<br>- Dynamic watermarking                                                                           |
| [StegaStamp: Invisible Hyperlinks in Physical Photographs](https://ieeexplore.ieee.org/document/9156548)                                                             | CVPR          | 2020                      | [code](https://github.com/tancik/StegaStamp)                  | [Arxiv](https://arxiv.org/abs/1904.05343)     | - "Towards the Vulnerability of Watermarking Artificial Intelligence Generated Content" speculates that [Deepmind SynthID](https://deepmind.google/technologies/synthid/) works similarly to this |
| [Glaze: Protecting artists from style mimicry by text-to-image models](https://www.usenix.org/conference/usenixsecurity23/presentation/shan)                         | USENIX        | 2023                      | [code](https://github.com/EspacioLatente/Glaze.git)           | [Arxiv](https://arxiv.org/abs/2302.04222)     | - Is not about watermarking a model, but denying style stealing                                                                                                                                   |
| DUAW: Data-free Universal Adversarial Watermark against Stable Diffusion Customization                                                                               | -             | 2023                      | -                                                             | [Arxiv](https://arxiv.org/abs/2308.09889)     | - Seem similar to Glaze on first glance. Authors may have been unlucky to do parallel work                                                                                                        |
| [Artificial Fingerprinting for Generative Models: Rooting Deepfake Attribution in Training Data](https://ieeexplore.ieee.org/document/9711167)                       | ICCV          | 2021                      | -                                                             | [Arxiv](https://arxiv.org/abs/2007.08457)     | - Watermarking GAN models by embedding watermark into training data to exploit <br>transferability</br>                                                                                           |
| Invisible Image Watermarks Are Provably Removable Using Generative AI                                                                                                | -             | 2023                      | [code](https://github.com/XuandongZhao/WatermarkAttacker.git) | [Arxiv](https://arxiv.org/abs/2306.01953)     | - Is not about watermarking a model, but removing watermarks<br>- Evaluates Tree-Ring Watermarks. Tree-ring is robust against their attack.                                                       |
| [Towards Blind Watermarking: Combining Invertible and Non-invertible Mechanisms](https://dl.acm.org/doi/abs/10.1145/3503161.3547950)                                 | MM            | 2022                      | [code](https://github.com/rmpku/CIN)                          | [Arxiv](https://arxiv.org/abs/2212.12678)     | - Is not about watermarking a model, but about attacking post-hoc watermarking of images<br>- Lots of references on invertible NNs                                                                |
| [DocDiff: Document Enhancement via Residual Diffusion Models](https://dl.acm.org/doi/abs/10.1145/3581783.3611730)                                                    | MM            | 2023                      | [code](https://github.com/Royalvice/DocDiff)                  | [Arxiv](https://arxiv.org/abs/2305.03892)     | - Is not about watermarking a model, but about post-hoc watermarking of images<br>- Includes classic watermark removal                                                                            |
| Warfare:Breaking the Watermark Protection of AI-Generated Content                                                                                                    | -             | 2023                      | Did not look for it yet                                       | [Arxiv](https://arxiv.org/abs/2310.07726)     | - Is not about watermarking a model, but about attacking post-hoc watermarking<br>- Includes 1. watermark removal and 2. forging                                                                  |
| [Leveraging Optimization for Adaptive Attacks on Image Watermarks](https://openreview.net/forum?id=O9PArxKLe1)                                                       | ICML (Poster) | 2024                      | Did not look for it yet                                       | [Arxiv](https://arxiv.org/abs/2309.16952v2)   | - Is not about watermarking a model, but about attacking post-hoc watermarking                                                                                                                    |
| A Somewhat Robust Image Watermark against Diffusion-based Editing Models                                                                                             | -             | 2023                      | Did not look for it yet                                       | [Arxiv](https://arxiv.org/abs/2311.13713)     | - Is not about watermarking a model, but about post-hoc watermarking of images<br>- Takes watermarks literally and injects hidden images<br>- Low priority                                        |
| Hey That's Mine Imperceptible Watermarks are Preserved in Diffusion Generated Outputs                                                                                | -             | 2023                      | -                                                             | [Arxiv](https://arxiv.org/abs/2308.11123)     | - Is not about watermarking a model. They show that watermarks in training data are recognizable in output and allow for intellectual property claims                                             |
| Benchmarking the Robustness of Image Watermarks                                                                                                                      | -             | 2024                      | [code](https://wavesbench.github.io/)                         | [Arxiv](https://arxiv.org/abs/2401.08573)     | - Just a benchmark/framework for testing watermarks against                                                                                                                                       |
| [Free Fine-tuning: A Plug-and-Play Watermarking Scheme for Deep Neural Networks](https://dl.acm.org/doi/10.1145/3581783.3612331))                                    | MM            | 2023                      | Did not look for it yet                                       | Did not look for it yet                       | -                                                                                                                                                                                                 |
| [Share on Adversarial Attack for Robust Watermark Protection Against Inpainting-based and Blind Watermark Removers](https://dl.acm.org/doi/10.1145/3581783.3612034)) | MM            | 2023                      | Did not look for it yet                                       | Did not look for it yet                       | -                                                                                                                                                                                                 |
| [A Novel Deep Video Watermarking Framework with Enhanced Robustness to H.264/AVC Compression](https://dl.acm.org/doi/10.1145/3581783.3612270))                       | MM            | 2023                      | Did not look for it yet                                       | Did not look for it yet                       | -                                                                                                                                                                                                 |
| [Practical Deep Dispersed Watermarking with Synchronization and Fusion](https://dl.acm.org/doi/10.1145/3581783.3612015))                                             | MM            | 2023                      | Did not look for it yet                                       | Did not look for it yet                       | -                                                                                                                                                                                                 |
| Generalizable Synthetic Image Detection via Language-guided Contrastive Learning                                                                                     | -             | 2023                      | [code](https://github.com/HighwayWu/LASTED)                   | [Arxiv](https://arxiv.org/abs/2305.13800)     | - Is on GenAI image detection, not watermarking                                                                                                                                                   |

TODO: Need to add categories regarding the type of model which is watermarked.
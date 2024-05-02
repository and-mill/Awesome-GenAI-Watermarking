# Awesome-GenAI-Watermarking

This repo include papers about the watermarking methods for generative AI models.
Watermarking is a method for embedding an imperceptible, but recoverable signal (payload) into a digital asset (cover).
With generative models, there are approaches which train the model to produce the watermark in every output and this behaviour should be hard to disable. We refer to this as **"Fingerprint Rooting"** or just **"Rooting"**. 

- [1. Introduction](#1-introduction)
  - [1.1 Watermarking Goals](#11-watermarking-goals)
  - [1.2 Differences Between Watermarking Schemes](#12-differences-between-watermarking-schemes)
  - [1.3 What Information is Transported by the Watermark? ](#13-what-information-is-transported-by-the-watermark-)
  - [1.4 Attacks on Watermarking](#14-attacks-on-watermarking)
- [2. Image Domain](#2-image-domain)
  - [2.1. Papers on Watermarking Diffusion Models Image](#21-papers-on-watermarking-diffusion-models-outputs-image)
  - [2.3. Tree-Ring like](#22-tree-ring-like)
  - [2.3. Misc Papers (to Be Categorized)](#23-misc-papers-to-be-categorized)
- [3. Audio Domain](#3-audio-domain)
  - [3.1. Papers on Watermarking Audio](#31-papers-on-watermarking-audio)
  - [3.2. Audio Synthesis Datasets](#32-audio-synthesis-datasets)
  - [3.3. News on Audio](#33-news-on-audio-watermarking)
  - [3.4. Further Links on Audio Synthesis and Detection](#34-further-links-on-audio-synthesis-and-detection)
- [4. Text Domain](#4-text-domain) 
- [4. Related News](#5-related-news)
- [5. Generative Model stealing Papers](#6-generative-model-stealing-papers)
- [6. Survey Papers](#7-survey-papers)
- [6.1 A Systematic Review on Model Watermarking for Neural Networks](#71-a-systematic-review-on-model-watermarking-for-neural-networks)

<!--####################################################################################################-->
<!--INTRODUCTION-->
<!--####################################################################################################-->
# 1. Introduction

---
## 1.1 Watermarking Goals
- **Deep fake detection** (Is a digit asset AI-generated?)
  - Counter misinformation
  - Prevent data crawlers from picking up generated content and cause "[Self-Consuming Generative Models Go MAD](https://openreview.net/forum?id=ShjMHfmPs0)"
- **Deep fake attribution** (By whom (which user of A model API) has it been generated?)
- **Enhanced Model Fingerprinting** (By which model has it been generated?)
- **IP protection**
  - Protect valuable models
  - Protect valuable training data (e.g. style)
- **Tamper Localization** (Where has an asset been doctored?)
  - see "EditGuard: Versatile Image Watermarking for Tamper Localization and Copyright Protection"

## 1.2 Differences Between Watermarking Schemes
- Fingerprint Rooting vs. Post-Hoc Watermarking
  - Fingerprint Rooting
    - The model is manipulated in a way that its usual generating process produces watermarked output
    - This DOES protect open models
  - Post-Hoc Watermarking
    - The Watermark is added after the fact, in a separate process
    - This does NOT protect open models, as the watermark embedding procedure can be simply disabled
- Static vs. Dynamic Watermarking
  - Static watermarking
    - "[...] specific pattern in its static content, such as a particular distribution of parameters" (see "Intellectual Property Protection of Diffusion Models via the Watermark Diffusion Process")
  - Dynamic Watermarking
    - "[...] specific pattern in model’s dynamic contents, such as its behavior.", e.g. trigger-prompt-watermark-backdoors
      - Requires at least API-access to the model, as the watermark will only be present upon specific trigger
      - Examples for introducing backdoors in general (not for watermarking specifically) into diffusion models:
        - [TrojDiff: Trojan Attacks on Diffusion Models with Diverse Targets](https://arxiv.org/abs/2303.05762)
        - [How to Backdoor Diffusion Models?](https://arxiv.org/abs/2212.05400)
- Tuning mechanism
  - Via Training data
    - e.g. [Artificial Fingerprinting for Generative Models: Rooting Deepfake Attribution in Training Data](https://ieeexplore.ieee.org/document/9711167)) -> "Plug-and-play", as it works on all architectures due to transferability
  - Via joint fine-tuning of a model and a decoder (taken from a encoder/decoder-pair) on few samples
    - e.g. [The Stable Signature: Rooting Watermarks in Latent Diffusion Models](https://openaccess.thecvf.com/content/ICCV2023/html/Fernandez_The_Stable_Signature_Rooting_Watermarks_in_Latent_Diffusion_Models_ICCV_2023_paper.html) (Requires latent generative model)
- Flexibility: How quick can a model be rooted:
  - Full training for each instance
    - e.g. [Artificial Fingerprinting for Generative Models: Rooting Deepfake Attribution in Training Data](https://ieeexplore.ieee.org/document/9711167)
  - Fine-tuning for each instance
    - e.g. [The Stable Signature: Rooting Watermarks in Latent Diffusion Models](https://openaccess.thecvf.com/content/ICCV2023/html/Fernandez_The_Stable_Signature_Rooting_Watermarks_in_Latent_Diffusion_Models_ICCV_2023_paper.html)
  - Fine-tuning once
    - e.g. using message matrix as in [Flexible and Secure Watermarking for Latent Diffusion Model](https://dl.acm.org/doi/10.1145/3581783.3612448)

## 1.3 What Information is Transported by the Watermark? 
- Generated asset yes/no
- Identity of watermarking party
- Identifier of the asset in provenance database (can replace perceptual hashing, mentioned in "RoSteALS: Robust Steganography using Autoencoder Latent Space")

## 1.4 Attacks on Watermarking
- Watermark removal
  - Removing a watermark from a given digital asset
  - Attacker goals
    - A fake asset can be claimed to be real
      - Disable IP claims
      - Misinformation
      - Sensor spoofing (weird, but it was mentioned in [Responsible Disclosure of Generative Models Using Scalable Fingerprinting](https://openreview.net/forum?id=sOK-zS6WHB))
  - Robustness property
    - Removing the watermark should decrease the asset quality. This negates the usefulness of the asset for malicious goals
- Watermark forgery (referred to as spoofing by [Robustness of AI-Image Detectors: Fundamental Limits and Practical Attacks](https://openreview.net/forum?id=dLoAdIKENc&referrer=%5Bthe%20profile%20of%20Soheil%20Feizi%5D(%2Fprofile%3Fid%3D~Soheil_Feizi2)))
  - Adding a watermark to a given digital asset
  - Attacker goals
    - False IP claims
    - A real asset can be denounced as fake (Misinformation)
    - Reputation loss after linking obscene content with model (mentioned in [Robustness of AI-Image Detectors: Fundamental Limits and Practical Attacks](https://openreview.net/forum?id=dLoAdIKENc&referrer=%5Bthe%20profile%20of%20Soheil%20Feizi%5D(%2Fprofile%3Fid%3D~Soheil_Feizi2)))
- Model purification
  - A watermarked model which should only produce watermarked output, even if distributed to untrusted parties (i.e. Stable Signature), is "purified" in a way that removes the watermarks in its output.
  - Attacker goals
    - Obtain a model which does not produced watermarked content
  - Robustness property
    - Removing the watermark functionality of the model should decrease the output quality. This negates the usefulness of the asset for malicious goals

### Threat models
- Whitebox
  - Attacker has full access to a generative AI model
- ... TODO

<!--####################################################################################################-->
<!--IMAGE DOMAIN-->
<!--####################################################################################################-->
# 2. Image Domain

---
## 2.1 Papers on Watermarking Diffusion Models (outputs) (Image)
| Paper                                                                                                                                                                                                                               | Proceedings/Journal                  | Venue Year / Last Updated | Code                                                               | Alternative PDF Source                                   | Notes                                                                                                                                                                                                                                                                                                                                     |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|---------------------------|--------------------------------------------------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Robust Image Watermarking using Stable Diffusion                                                                                                                                                                                    | -                                    | 2024                      | -                                                                  | [Arxiv](https://arxiv.org/abs/2401.04247)                | - Very similar to Tree-Ring                                                                                                                                                                                                                                                                                                               |
| [PTW: Pivotal Tuning Watermarking for Pre-Trained Image Generators](https://www.usenix.org/conference/usenixsecurity23/presentation/lukas)                                                                                          | USENIX                               | 2023                      | [Github](https://github.com/nilslukas/gan-watermark)               | [Arxiv](https://arxiv.org/abs/2304.07361)                | - Focus on GANs, but latent diffusion models should work too                                                                                                                                                                                                                                                                              |
| [The Stable Signature: Rooting Watermarks in Latent Diffusion Models](https://openaccess.thecvf.com/content/ICCV2023/html/Fernandez_The_Stable_Signature_Rooting_Watermarks_in_Latent_Diffusion_Models_ICCV_2023_paper.html)        | ICCV                                 | 2023                      | [Github](https://github.com/facebookresearch/stable_signature.git) | [Arxiv](https://arxiv.org/abs/2303.15435)                | - Meta/FAIR author<br>Finetune a model in accordance with encoder/decoder to reveal a secret message in its output.<br>- robust to watermark removal and model purification (quality deterioration)<br>- Static watermarking                                                                                                              |
| [Flexible and Secure Watermarking for Latent Diffusion Model](https://dl.acm.org/doi/10.1145/3581783.3612448) (behind paywall with no arxiv, need university IP address to get this PDF)                                            | ACM MM                               | 2023                      | -                                                                  | -                                                        | - References Stable Signature and improves by adding flexibility by allowing for embedding different messages w.o. finetuning                                                                                                                                                                                                             |
| A Training-Free Plug-and-Play Watermark Framework for Stable Diffusion                                                                                                                                                              | -                                    | 2024                      | -                                                                  | [Arxiv](https://arxiv.org/abs/2404.05607)                | - TODO                                                                                                                                                                                                                                                                                                                                    |
| [WOUAF: Weight Modulation for User Attribution and Fingerprinting in Text-to-Image Diffusion Models](https://openreview.net/forum?id=IMU-LkRKki&referrer=%5Bthe%20profile%20of%20Sheng%20Cheng%5D(%2Fprofile%3Fid%3D~Sheng_Cheng1)) | NeurIPS Workshop on Diffusion Models | 2023                      | -                                                                  | [Arxiv](https://arxiv.org/abs/2306.04744)                | - TODO                                                                                                                                                                                                                                                                                                                                    |
| [RoSteALS: Robust Steganography using Autoencoder Latent Space](https://ieeexplore.ieee.org/document/10208817)                                                                                                                      | CVPR Workshops (CVPRW)               | 2023                      | [Github](https://github.com/coriverchen/Robust_Steganography.git)  | [Arxiv](https://arxiv.org/abs/2304.03400)                | - Post-hoc watermarking                                                                                                                                                                                                                                                                                                                   |
| [DiffusionShield: A Watermark for Copyright Protection against Generative Diffusion Models](https://neurips.cc/virtual/2023/74895)                                                                                                  | NeurIPS Workshop on Diffusion Models | 2023                      | -                                                                  | [Arxiv](https://arxiv.org/abs/2306.04642)                | - Not about Rooting<br>-Data Poisoning protected images which will reproduce if used as training data in diffusion model                                                                                                                                                                                                                  |
| A Recipe for Watermarking Diffusion Models                                                                                                                                                                                          | -                                    | 2023                      | [Github](https://github.com/yunqing-me/WatermarkDM.git)            | [Arxiv](https://arxiv.org/abs/2303.10137)                | - Framework for 1. small unconditional/class-conditional DMs via training from scratch on watermarked data and 2. text-to-image DMs via finetuning a backdoor-trigger-output<br>- Lots of references on watermarking discriminative models<br>- Static watermarking                                                                       |
| Intellectual Property Protection of Diffusion Models via the Watermark Diffusion Process                                                                                                                                            | -                                    | 2023                      | -                                                                  | [Arxiv](https://arxiv.org/abs/2306.03436)                | - Threat model: Check ownership of model by having access to the model<br>- Hard to read<br>- Explains difference between **static and dynamic watermarking** with many references                                                                                                                                                        |
| Securing Deep Generative Models with Universal Adversarial Signature                                                                                                                                                                | -                                    | 2023                      | [Github](https://github.com/zengxianyu/genwm)                      | [Arxiv](https://arxiv.org/abs/2305.16310)                | - 1. Find optimal signature for an image individually.<br>- 2. Finetune a GenAI model on these images.                                                                                                                                                                                                                                    |
| Watermarking Diffusion Model                                                                                                                                                                                                        | -                                    | 2023                      | -                                                                  | [Arxiv](https://arxiv.org/abs/2305.12502)                | - Finetuning a backdoor-trigger-output<br>- Static watermarking<br>- CISPA authors                                                                                                                                                                                                                                                        |
| Catch You Everything Everywhere: Guarding Textual Inversion via Concept Watermarking                                                                                                                                                | -                                    | 2023                      | -                                                                  | [Arxiv](https://arxiv.org/abs/2309.05940)                | - Guards concepts obtained through textual inversion ([An Image is Worth One Word: Personalizing Text-to-Image Generation using Textual Inversion](https://arxiv.org/abs/2208.01618)) from abuse by allowing to identify concepts in generated images.<br>- Very interesting references on company and government stances on watermarking |
| Generative Watermarking Against Unauthorized Subject-Driven Image Synthesis                                                                                                                                                         | -                                    | 2023                      | -                                                                  | [Arxiv](https://arxiv.org/abs/2306.07754)                | - Different from Glaze in that style synthesis from protected source images is not prevented, but recognizable via watermarks<br>- CISPA authors                                                                                                                                                                                          |
| Towards the Vulnerability of Watermarking Artificial Intelligence Generated Content                                                                                                                                                 | -                                    | 2024                      | -                                                                  | [OpenReview](https://openreview.net/forum?id=xY4861TVUc) | - Watermark removal and forgery in one method, using GAN<br>- References two types of watermarking: **1. Learn/finetune model to produce watermarked output and 2. post-hoc watermarking after the fact** (static vs. dynamic, see "Intellectual Property Protection of Diffusion Models via the Watermark Diffusion Process")            |
| [Robustness of AI-Image Detectors: Fundamental Limits and Practical Attacks](https://openreview.net/forum?id=dLoAdIKENc&referrer=%5Bthe%20profile%20of%20Soheil%20Feizi%5D(%2Fprofile%3Fid%3D~Soheil_Feizi2))                       | ICLR (Poster)                        | 2024                      | [Github](https://github.com/mehrdadsaberi/watermark_robustness)    | [Arxiv](https://arxiv.org/abs/2310.00076)                | - They show that low budget watermarking methods are beaten by diffusion purification and propose an attack that can even remove high budget watermarks by model substitution                                                                                                                                                             |
| A Transfer Attack to Image Watermarks                                                                                                                                                                                               | -                                    | 2024                      | -                                                                  | [Arxiv](https://arxiv.org/abs/2403.15365)                | - Watermark removal by "no-box"-attack on detectors (no access to detector-API, instead training classifier to distinguish watermarked and vanilla images)                                                                                                                                                                                |
| EditGuard: Versatile Image Watermarking for Tamper Localization and Copyright Protection                                                                                                                                            | -                                    | 2023                      | [Github](https://github.com/xuanyuzhang21/EditGuard)               | [Arxiv](https://arxiv.org/abs/2312.08883)                | - Post-hoc watermarking with tamper localization                                                                                                                                                                                                                                                                                          |
| Latent Watermark: Inject and Detect Watermarks in Latent Diffusion Space                                                                                                                                                            | -                                    | 2024                      | -                                                                  | [Arxiv](https://arxiv.org/abs/2404.00230)                | - Discusses 3 categories for watermarks with references: before, during, and after generation                                                                                                                                                                                                                                             |
| Stable Messenger: Steganography for Message-Concealed Image Generation                                                                                                                                                              | -                                    | 2023                      | -                                                                  | [Arxiv](https://arxiv.org/abs/2312.01284)                | - Post-hoc watermarking<br>- Watermark embedding during generation according to "Latent Watermark: Inject and Detect Watermarks in Latent Diffusion Space", but I think it is actually post-hoc.                                                                                                                                          |

## 2.2 Tree-Ring-Like
| Paper                                                                                                                                                                                                                               | Proceedings/Journal | Venue Year / Last Updated | Code                                                               | Alternative PDF Source                                   | Notes                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|---------------------------|--------------------------------------------------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Tree-Ring Watermarks: Fingerprints for Diffusion Images that are Invisible and Robust](https://proceedings.neurips.cc/paper_files/paper/2023/hash/b54d1757c190ba20dbc4f9e4a2f54149-Abstract-Conference.html)                       | NeurIPS             | 2023                      | [Github](https://github.com/YuxinWenRick/tree-ring-watermark)      | [Arxiv](https://arxiv.org/abs/2305.20030)                | - Embedding tree-ring into FFT of noise vector<br>- Not about Rooting, but manipulating the noise vector before using vanilla stable diffusion        |
| RingID: Rethinking Tree-Ring Watermarking for Enhanced Multi-Key Identification                       | -                   | 2024                      | [Github](https://github.com/showlab/RingID)      | [Arxiv](https://arxiv.org/abs/2404.14055)                | - Adding capability to distinguish more IDs.<br/>- Attribution success of original Tree-Rings to distribution shift instead of Tree-Ring pattern only |

## 2.3 Misc Papers (to be categorized...)
| Paper                                                                                                                                                                                           | Proceedings/Journal         | Venue Year / Last Updated | Code                                                            | Alternative PDF Source                      | Notes                                                                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------|---------------------------|-----------------------------------------------------------------|---------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gaussian Shading: Provable Performance-Lossless Image Watermarking for Diffusion Models                                                                                                         | -                           | 2024                      | -                                                               | [Arxiv](https://arxiv.org/abs/2404.04956)   | - Has nice overview of different types of watermarking (Post-Hox, Finetuning, Latent Imprinting)<br/>- Contains motivation example about some important fake news incident                                           |
| [WaterDiff: Perceptual Image Watermarks Via Diffusion Model](https://ieeexplore.ieee.org/abstract/document/10447095)                                                                            | IVMSP-P2 Workshop at ICASSP | 2024                      | -                                                               | -                                           | - TODO                                                                                                                                                                                                               |
| [Squint Hard Enough: Attacking Perceptual Hashing with Adversarial Machine Learning](https://www.usenix.org/conference/usenixsecurity23/presentation/prokos)                                    | USENIX                      | 2022                      | -                                                               | -                                           | - Attacks on perceptual hashes                                                                                                                                                                                       |
| [Evading Watermark based Detection of AI-Generated Content](https://dl.acm.org/doi/10.1145/3576915.3623189)                                                                                     | CCS                         | 2023                      | [Github](https://github.com/zhengyuan-jiang/WEvade)             | [Arxiv](https://arxiv.org/abs/2305.03807)   | - Evaluation of robustness of image watermarks + Adversarial sample for evasion                                                                                                                                      |
| [Diffusion Models for Adversarial Purification](https://proceedings.mlr.press/v162/nie22a.html)                                                                                                 | ICML                        | 2022                      | [Github](https://github.com/NVlabs/DiffPure)                    | [Arxiv](https://arxiv.org/abs/2205.07460)   | - Defense against adversarial pertubation, including imperceptible watermarks in images                                                                                                                              |
| [HiDDeN: Hiding Data With Deep Networks](https://dl.acm.org/doi/abs/10.1007/978-3-030-01267-0_40)                                                                                               | ECCV                        | 2018                      | [Github](https://github.com/jirenz/HiDDeN)                      | [Arxiv](https://arxiv.org/abs/1807.09937)   | - Main tool used in Stable Signature<br>- Contains differentiable approx. of JPEG compression<br>- Dynamic watermarking                                                                                              |
| [StegaStamp: Invisible Hyperlinks in Physical Photographs](https://ieeexplore.ieee.org/document/9156548)                                                                                        | CVPR                        | 2020                      | [Github](https://github.com/tancik/StegaStamp)                  | [Arxiv](https://arxiv.org/abs/1904.05343)   | - "Towards the Vulnerability of Watermarking Artificial Intelligence Generated Content" speculates that [Deepmind SynthID](https://deepmind.google/technologies/synthid/) works similarly to this                    |
| [Glaze: Protecting artists from style mimicry by text-to-image models](https://www.usenix.org/conference/usenixsecurity23/presentation/shan)                                                    | USENIX                      | 2023                      | [Github](https://github.com/EspacioLatente/Glaze.git)           | [Arxiv](https://arxiv.org/abs/2302.04222)   | - Is not about Rooting, but denying style stealing                                                                                                                                                                   |
| DUAW: Data-free Universal Adversarial Watermark against Stable Diffusion Customization                                                                                                          | -                           | 2023                      | -                                                               | [Arxiv](https://arxiv.org/abs/2308.09889)   | - Seem similar to Glaze on first glance. Authors may have been unlucky to do parallel work                                                                                                                           |
| [Artificial Fingerprinting for Generative Models: Rooting Deepfake Attribution in Training Data](https://ieeexplore.ieee.org/document/9711167)                                                  | ICCV                        | 2021                      | -                                                               | [Arxiv](https://arxiv.org/abs/2007.08457)   | - Rooting GAN models. By embedding watermark into training data to exploit <br>transferability</br>                                                                                                                  |
| [Responsible Disclosure of Generative Models Using Scalable Fingerprinting](https://openreview.net/forum?id=sOK-zS6WHB)                                                                         | ICLR                        | 2022                      | [Github](https://github.com/ningyu1991/ScalableGANFingerprints) | [Arxiv](https://arxiv.org/abs/2012.08726)   | - Rooting GAN models. Seems to have introduced the idea of scalably producing many models fast with large message space (TODO: check this later), similar to how Stable Signature did it later for stable diffusion. |
| On Attribution of Deepfakes                                                                                                                                                                     | -                           | 2020                      | -                                                               | [Arxiv](https://arxiv.org/abs/2008.09194)   | - They show that an image can be created that looks like it may have been generated by a targeted model. They also propose a framework how to achieve deniability for such cases.                                    |
| Invisible Image Watermarks Are Provably Removable Using Generative AI                                                                                                                           | -                           | 2023                      | [Github](https://github.com/XuandongZhao/WatermarkAttacker.git) | [Arxiv](https://arxiv.org/abs/2306.01953)   | - Is not about rooting a model, but removing watermarks with diffusion purification<br>- Evaluates stable signature and Tree-Ring Watermarks. Tree-ring is robust against their attack.                              |
| [Towards Blind Watermarking: Combining Invertible and Non-invertible Mechanisms](https://dl.acm.org/doi/abs/10.1145/3503161.3547950)                                                            | ACM MM                      | 2022                      | [Github](https://github.com/rmpku/CIN)                          | [Arxiv](https://arxiv.org/abs/2212.12678)   | - Is not about rooting a model, but about attacking post-hoc watermarking of images<br>- Lots of references on invertible NNs                                                                                        |
| [DocDiff: Document Enhancement via Residual Diffusion Models](https://dl.acm.org/doi/abs/10.1145/3581783.3611730)                                                                               | ACM MM                      | 2023                      | [Github](https://github.com/Royalvice/DocDiff)                  | [Arxiv](https://arxiv.org/abs/2305.03892)   | - Is not about rooting a model, but about post-hoc watermarking of images<br>- Includes classic watermark removal                                                                                                    |
| Warfare:Breaking the Watermark Protection of AI-Generated Content                                                                                                                               | -                           | 2023                      | Did not look for it yet                                         | [Arxiv](https://arxiv.org/abs/2310.07726)   | - Is not about rooting a model, but about attacking post-hoc watermarking<br>- Includes 1. watermark removal and 2. forging                                                                                          |
| [Leveraging Optimization for Adaptive Attacks on Image Watermarks](https://openreview.net/forum?id=O9PArxKLe1)                                                                                  | ICML (Poster)               | 2024                      | Did not look for it yet                                         | [Arxiv](https://arxiv.org/abs/2309.16952v2) | - Is not about rooting a model, but about attacking post-hoc watermarking                                                                                                                                            |
| A Somewhat Robust Image Watermark against Diffusion-based Editing Models                                                                                                                        | -                           | 2023                      | Did not look for it yet                                         | [Arxiv](https://arxiv.org/abs/2311.13713)   | - Is not about rooting a model, but about post-hoc watermarking of images<br>- Takes watermarks literally and injects hidden images                                                                                  |
| Hey That's Mine Imperceptible Watermarks are Preserved in Diffusion Generated Outputs                                                                                                           | -                           | 2023                      | -                                                               | [Arxiv](https://arxiv.org/abs/2308.11123)   | - Is not about rooting a model. They show that watermarks in training data are recognizable in output and allow for intellectual property claims                                                                     |
| Benchmarking the Robustness of Image Watermarks                                                                                                                                                 | -                           | 2024                      | [Github](https://wavesbench.github.io/)                         | [Arxiv](https://arxiv.org/abs/2401.08573)   | - Just a benchmark/framework for testing watermarks against                                                                                                                                                          |
| [Free Fine-tuning: A Plug-and-Play Watermarking Scheme for Deep Neural Networks](https://dl.acm.org/doi/10.1145/3581783.3612331)                                                                | ACM MM                      | 2023                      | Did not look for it yet                                         | [Arxiv](https://arxiv.org/abs/2210.07809)   | - Is not about generative models, but discriminative models                                                                                                                                                          |
| [Adversarial Attack for Robust Watermark Protection Against Inpainting-based and Blind Watermark Removers](https://dl.acm.org/doi/10.1145/3581783.3612034)                                      | ACM MM                      | 2023                      | Did not look for it yet                                         | -                                           | - Post-hoc watermark with enhanced robustness against inpainting                                                                                                                                                     |
| [A Novel Deep Video Watermarking Framework with Enhanced Robustness to H.264/AVC Compression](https://dl.acm.org/doi/10.1145/3581783.3612270)                                                   | ACM MM                      | 2023                      | [Github](https://github.com/yulinsysu/REVMark)                  | -                                           | - Post-hoc watermark for videos                                                                                                                                                                                      |
| [Practical Deep Dispersed Watermarking with Synchronization and Fusion](https://dl.acm.org/doi/10.1145/3581783.3612015)                                                                         | ACM MM                      | 2023                      | Did not look for it yet                                         | [Arxiv](https://arxiv.org/abs/2310.14532)   | - Post-hoc watermark for images with enhanced robustness to transformations                                                                                                                                          |
| Generalizable Synthetic Image Detection via Language-guided Contrastive Learning                                                                                                                | -                           | 2023                      | [Github](https://github.com/HighwayWu/LASTED)                   | [Arxiv](https://arxiv.org/abs/2305.13800)   | - Is not about rooting, but GenAI image detection                                                                                                                                                                    |
| [Enhancing the Robustness of Deep Learning Based Fingerprinting to Improve Deepfake Attribution](https://dl.acm.org/doi/abs/10.1145/3551626.3564981)                                            | ACM MM-Asia                 | 2022                      | -                                                               | -                                           | - Is not about rooting, but transformation-robustness strategies for watermarks                                                                                                                                      |
| [You are caught stealing my winning lottery ticket! Making a lottery ticket claim its ownership](https://proceedings.neurips.cc/paper/2021/hash/0dfd8a39e2a5dd536c185e19a804a73b-Abstract.html) | NeurIPS                     | 2021                      | [Github](https://github.com/VITA-Group/NO-stealing-LTH.)        | [Arxiv](https://arxiv.org/abs/2111.00162)   | - Watermarking the sparsity mask of winning lottery tickets                                                                                                                                                          |
| [Self-Consuming Generative Models Go MAD](https://openreview.net/forum?id=ShjMHfmPs0)                                                                                                           | ICLR (Poster)               | 2024                      | -                                                               | [Arxiv](https://arxiv.org/abs/2307.01850)   | - Contains a reason why GenAI detection is important: Removing generated content from training sets                                                                                                                  |

<!--####################################################################################################-->
<!--AUDIO DOMAIN-->
<!--####################################################################################################-->
# 3. Audio Domain

---
## 3.1 Papers on Watermarking (Audio)
| Paper                                                                                                                                                                                       | Proceedings/Journal | Venue Year / Last Updated | Code                                                                         | Alternative PDF Source                                                         | Notes                                                                                                                                                 |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|---------------------------|------------------------------------------------------------------------------|--------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| Proactive Detection of Voice Cloning with Localized Watermarking                                                                                                                            | -                   | 2024                      | [Github](https://github.com/facebookresearch/audioseal?tab=readme-ov-file)   | [Arxiv](https://arxiv.org/abs/2401.17264)                                      | - Meta/FAIR author                                                                                                                                    |
| [MaskMark: Robust Neural Watermarking for Real and Synthetic Speech](https://interactiveaudiolab.github.io/assets/papers/oreilly_jin_su_pardo_watermark.pdf)                                | ICASSP              | 2024                      | [Audio samples](https://interactiveaudiolab.github.io/project/maskmark.html) | [IEEExplore](https://ieeexplore.ieee.org/ielx7/10445798/10445803/10447253.pdf) | -                                                                                                                                                     |
| Collaborative Watermarking for Adversarial Speech Synthesis                                                                                                                                 | ICASSP              | 2024                      | -                                                                            | [Arxiv](https://arxiv.org/abs/2309.15224)                                      | - Meta/FAIR author                                                                                                                                    |
| [HiFi-GAN: Generative Adversarial Networks for Efficient and High Fidelity Speech Synthesis](https://proceedings.neurips.cc/paper/2020/hash/c5d736809766d46260d816d8dbc9eb44-Abstract.html) | NeurIPS             | 2020                      | [Github](https://github.com/jik876/hifi-gan)                                 | [Arxiv](https://arxiv.org/abs/2010.05646)                                      | - Very good GAN for Speech synthesis (TODO: Is this SotA?)<br/>- Can do live synthesis even on CPU<br/>- Quality is on par with autoregressive models |
| [Spoofed Training Data for Speech Spoofing Countermeasure Can Be Efficiently Created Using Neural Vocoders](https://ieeexplore.ieee.org/document/10094779)                                  | ICASSP              | 2023                      | -                                                                            | [Arxiv](https://arxiv.org/abs/2210.10570)                                      | - Include vocoder generated training data to enhance detection capabilities for countermeasures                                                       |

## 3.2 Audio Synthesis Datasets
| Paper                                                                                                                                                                                       | Proceedings/Journal | Venue Year / Last Updated | Code                                                                       | Alternative PDF Source                                                         | Notes                                                                                                                                                 |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|---------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| [ASVspoof 2021 Challenge](https://www.asvspoof.org/asvspoof2021/asvspoof2021_evaluation_plan.pdf)                                                                                           | -                   | 2021                      | [Github](https://github.com/asvspoof-challenge/2021)                       | [Arxiv](https://arxiv.org/abs/2109.00535)                                      | - Challenge for audio spoofing detection                                                                                                              |
| [ADD 2022: the first Audio Deep Synthesis Detection Challenge](https://ieeexplore.ieee.org/document/9746939)                                                                                | ICASSP              | 2022                      | [Github](https://github.com/asvspoof-challenge/2021)                       | [Arxiv](https://arxiv.org/abs/2202.08433)                                      | - [Official Chinese challenge website (NO HTTPS!)](http://addchallenge.cn/)                                                                           |
- For more (but older dataset see [Awesome-DeepFake-Learning](https://github.com/Billy1900/Awesome-DeepFake-Learning))

## 3.3 News on Audio Watermarking
- Microsoft: [Introducing the watermark algorithm for synthetic voice identification](https://techcommunity.microsoft.com/t5/ai-azure-ai-services-blog/introducing-the-watermark-algorithm-for-synthetic-voice/ba-p/3298548)

## 3.4 Further Links on Audio Synthesis and Detection
- [Github topics: Audio Synthesis](https://github.com/topics/audio-synthesis)
- [Github topic: Audio Deepfake Detection](https://github.com/topics/audio-deepfake-detection)
- [Awesome Deepfakes Detection](https://github.com/Daisy-Zhang/Awesome-Deepfakes-Detection?tab=readme-ov-file#spatiotemporal-based)
- [Awesome-DeepFake-Learning](https://github.com/Billy1900/Awesome-DeepFake-Learning)
- [Stable Audio: Fast Timing-Conditioned Latent Audio Diffusion](https://stability.ai/research/stable-audio-efficient-timing-latent-diffusion) (is supposedly also very fast, like HIFI-GAN)
- Meta AI: [VoiceBox](https://voicebox.metademolab.com/)
  - SotA Speech model with many functionalities (noise removal (barking), style transfer, ...)
- TTS libraries (including speech synthesis)
  - [Coqui-AI library](https://github.com/coqui-ai/TTS)
  - [Mimic3](https://github.com/MycroftAI/mimic3)
  - [Amphion-AI](https://github.com/open-mmlab/Amphion)
  - [Tortoise TTS](https://github.com/neonbjb/tortoise-tts)
    - pet project by some student
    - includes tools for detection

<!--####################################################################################################-->
<!--TEXT DOMAIN-->
<!--####################################################################################################-->
# 4. Text Domain
| Paper                                                                                                                                  | Proceedings/Journal | Venue Year / Last Updated | Code                                                            | Alternative PDF Source                     | Notes                                         |
|----------------------------------------------------------------------------------------------------------------------------------------|---------------------|---------------------------|-----------------------------------------------------------------|--------------------------------------------|-----------------------------------------------|
| Watermarks in the Sand: Impossibility of Strong Watermarking for Generative Models                                                     | -                   | 2023                      | [Github](https://github.com/hlzhang109/impossibility-watermark) | [Arxiv](https://arxiv.org/abs/2311.04378)  | -                                             |
| [Adversarial Watermarking Transformer: Towards Tracing Text Provenance with Data Hiding](https://ieeexplore.ieee.org/document/9519400) | S&P                 | 2021                      | [Github](https://github.com/S-Abdelnabi/awt)                    | [Arxiv](https://arxiv.org/abs/2009.03015)  | -                                             |
| Resilient Watermarking for LLM-Generated Codes                                                                                         | -                   | 2024                      | [Github Appendix](https://github.com/boutiquelee/ACW)           | [Arxiv](https://arxiv.org/abs/2402.07518)  | - Code                                        |
| Provably Robust Multi-bit Watermarking for AI-generated Text via Error Correction Code                                                 | -                   | 2024                      | -                                                               | [Arxiv](https://arxiv.org/abs/2401.16820)  | - Error correction                            |
| [Provable Robust Watermarking for AI-Generated Text](https://openreview.net/forum?id=SsmT8aO45L)                                       | ICLR                | 2024                      | [Github](https://github.com/XuandongZhao/Unigram-Watermark)     | [Arxiv](https://arxiv.org/abs/2306.17439)  | - Apparently good and robust LLM Watermarking |

<!--####################################################################################################-->
<!--RELATED NEWS-->
<!--####################################################################################################-->
# 5. Related News

---
- [Coalition for Content Provenance and Authenticity (C2PA)](https://c2pa.org/)
  - Is based on a trust model with signing authorities which certify signer of some digital asset through a chain of trust, similar to internet PKI. 
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
          - "Soft bindings are described using soft binding assertions such as via a perceptual hash computed from the digital content or a watermark embedded within the digital content. These soft bindings enable digital content to be matched even if the underlying bits differ, for example due to an asset rendition in a different resolution or encoding format. Additionally, should a C2PA manifest be removed from an asset, but a copy of that manifest remains in a provenance store elsewhere, the manifest and asset may be matched using available soft bindings."
            - This includes [ISCC - Content Codes](https://iscc.codes/)
          - -> allows authors and subsequent actors to sign assets and make
            this act publicly known to establish the asset's history.
- [Open-source tools for content authenticity and provenance](https://opensource.contentauthenticity.org/)
  - Uses manifests defined in [C2PA Specifications](https://c2pa.org/specifications/specifications/1.3/index.html)
  - Enables camera manufacturers to insert authenticity meta-data on-device at time of capture
- [Deepmind SynthID](https://deepmind.google/technologies/synthid/)
    - Images ([Vertex AI imagen](https://cloud.google.com/vertex-ai/generative-ai/docs/image/overview))
    - Audio ([Google DeepMind's Lyria](https://deepmind.google/discover/blog/transforming-the-future-of-music-creation/))
        - Transfer artist's styles to audio prompts (style transfer)
        - Most likely, the watermark will include the prompt used to generate audio (e.g. Artist name). This allows for
          copyright claims.
    - In Beta (as of 26 Mar 2024)
    - Closed off as can be (as of 26 Mar 2024)
- Google hosted a workshop in June 2023 ([Identifying and Mitigating the Security Risks of Generative AI](https://arxiv.org/abs/2308.14840)): "Watermarking was mentioned as a promising mitigation. They are robust when attacker has no access to detection algorithm"
- Watermarking is identified as tool for establishing trust in a post GenAI environment by big tech ([OpenAI moving AI governance forward statement](https://openai.com/blog/moving-ai-governance-forward), [Google "Our commitment to advancing bold and responsible AI, together"](https://blog.google/outreach-initiatives/public-policy/our-commitment-to-advancing-bold-and-responsible-ai-together/)) and government ([Biden-⁠Harris Administration Secures Voluntary Commitments from Leading Artificial Intelligence Companies to Manage the Risks Posed by AI ](https://www.whitehouse.gov/briefing-room/statements-releases/2023/07/21/fact-sheet-biden-harris-administration-secures-voluntary-commitments-from-leading-artificial-intelligence-companies-to-manage-the-risks-posed-by-ai/))
- Watermarks can already be easily inserted into Stable Diffusion models (This method [invisible-watermark repo](https://github.com/ShieldMnt/invisible-watermark) is referenced by the official [Stable Diffusion repo](https://github.com/CompVis/stable-diffusion?tab=readme-ov-file)). This is based on [Digital Watermarking and Steganography](https://dl.acm.org/doi/book/10.5555/1564551#)" (DwtDct and DwtDctSvd). Also see watermark option in the Stable Diffusion repo https://github.com/CompVis/stable-diffusion/blob/main/scripts/txt2img.py#L69.

<!--####################################################################################################-->
<!--GENERATIVE MODEL STEALING-->
<!--####################################################################################################-->
# 6. Generative Model stealing Papers

---
| Paper                                                                                                                                               | Proceedings/Journal | Venue Year / Last Updated | Code | Alternative PDF Source                    | Notes |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|---------------------------|------|-------------------------------------------|-------|
| [Stealing Machine Learning Models: Attacks and Countermeasures for Generative Adversarial Networks](https://dl.acm.org/doi/10.1145/3485832.3485838) | ACSAC               | 2021                      | -    | [Arxiv](https://arxiv.org/abs/2101.02069) | -     |
| [Model Extraction Attack and Defense on Deep Generative Models](https://iopscience.iop.org/article/10.1088/1742-6596/2189/1/012024/meta)            | Journal of Physics  | 2022                      | -    | -                                         | -     |
| Model Extraction and Defenses on Generative Adversarial Networks                                                                                    | -                   | 2021                      | -    | [Arxiv](https://arxiv.org/abs/2101.02069) | -     |

<!--####################################################################################################-->
<!--SURVEY PAPERS-->
<!--####################################################################################################-->
# 7. Survey Papers

---
| Paper                                                                                                                                                  | Proceedings/Journal   | Venue Year / Last Updated | Code | Alternative PDF Source                    | Notes                                                                                                         |
|--------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|---------------------------|------|-------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| [A Comprehensive Survey on Robust Image Watermarking](https://www.sciencedirect.com/science/article/pii/S0925231222002533?ref=cra_js_challenge&fr=njs) | Neurocomputing        | 2022                      | -    | [Arxiv](https://arxiv.org/abs/2207.06909) | - Not about model rooting                                                                                     |
| [A Systematic Review on Model Watermarking for Neural Networks](https://www.frontiersin.org/articles/10.3389/fdata.2021.729663/full)                   | Frontiers in Big Data | 2021                      | -    | [Arxiv](https://arxiv.org/abs/2009.12153) | - Not about model rooting                                                                                     |
| A Comprehensive Review on Digital Image Watermarking                                                                                                   | -                     | 2022                      | -    | [Arxiv](https://arxiv.org/abs/2207.06909) | - Not about model rooting                                                                                     |
| Copyright Protection in Generative AI: A Technical Perspective                                                                                         | -                     | 2024                      | -    | [Arxiv](https://arxiv.org/abs/2402.02333) | - About IP protection in GenAI in general                                                                     |
| Security and Privacy on Generative Data in AIGC: A Survey                                                                                              | -                     | 2023                      | -    | [Arxiv](https://arxiv.org/abs/2309.09435) | - About security aspects in GenAI in general                                                                  |
| Detecting Multimedia Generated by Large AI Models: A Survey                                                                                            | -                     | 2024                      | -    | [Arxiv](https://arxiv.org/abs/2402.00045) | - About detecting GenAI in general                                                                            |
| Audio Deepfake Detection: A Survey                                                                                                                     | -                     | 2023                      | -    | [Arxiv](https://arxiv.org/abs/2308.14970) | - Contains overview of spoofed audio datasets, spoofing methods, and detection methods<br/>- Very good servey |

## 7.1 [A Systematic Review on Model Watermarking for Neural Networks](https://www.frontiersin.org/articles/10.3389/fdata.2021.729663/full)
Summarization of the systematization given in this review.
### Taxonomy
- Embedding method
  - Watermark in model parameters
  - Trigger-Watermark-Backdoor
- Verification access
  - Whitebox (access model parameters)
  - Blackbox (access via API)
- Capacity
  - Zero-bit (is watermark exists)
  - Multi-Bit (watermark contains arbitrary info)
- Authentication
  - Model is watermarked
  - By whom model is watermarked
- Uniqueness
  - All model instances carry same watermark
  - Different model instances carry different watermarks

### Requirements & Security Goals
| Goal        | Explaination                               | Motivation                                        |
|-------------|--------------------------------------------|---------------------------------------------------|
| Fidelity    | High prediction quality on original tasks  | model performance shouldn't significantly degrade |
| Robustness  | Watermark should resist removal            | protects against copyright evasion                |
| Reliability | Minimal false negatives                    | ensures rightful ownership is recognized          |
| Integrity   | Minimal false positives                    | prevents wrongful accusations of theft            |
| Capacity    | Supports large information amounts         | allows comprehensive watermarks                   |
| Secrecy     | Watermark must be secret and undetectable  | prevents unauthorized detection                   |
| Efficiency  | Fast watermark insertion and verification  | avoids computational burden                       |
| Generality  | Independent of datasets and ML algorithms  | facilitates widespread application                |

### Threat Model
- Attacker Knowledge:
  1. existence of the watermark
  2. model and its parameters
  3. watermarking scheme used
  4. (parts of) the training data
  5. (parts of) the watermark itself or the trigger dataset
- Attacker Capabilities (irrelevant)
  - passive (eavesdropping)
  - active (interaction)
- Attacker Objectives
  - For what is model being used by the attacker? (rather unspecific)

### Attacks against Watermarking
- Watermark Detection (weakest)
- Watermark Suppression, i.e. avoid watermark verification
  - e.g. dissimulating any presence of a watermark in the model parameters and behavior
  - e.g. suppressing the reactions of the model to the original watermark trigger
- Watermark Forging
  1. Recovering the legitimate owner’s watermark and claiming ownership (if there is no binding between the watermark and the owner)
  2. Adding a new watermark that creates ambiguity concerning ownership
  3. Identifying a fake watermark within the model that coincidentally acts like a real watermark but actually is not
- Watermark Overwriting
  1. Adding Watermark to model with deactivating old one (strong)
  2. Adding Watermark to model without deactivating old one (weak)
- Watermark Removal
  1. depends on the presence of a watermark
  2. depends on the underlying watermarking scheme
  3. depends on availability of additional data, e.g. for fine-tuning or retraining
     - Methods
       - Fine-Tuning
       - Pruning 
       - Quantization
       - Distillation
       - Transfer-Learning
       - Backdoor Removal

### Categorizing Watermarking Methods
- Embedding Watermarks into Model Parameters
  - Adding patterns into model which can be verified locally
- Using Pre-Defined Inputs as Triggers
  - Adding behaviour triggered by special input
- Using Model Fingerprints to Identify Potentially Stolen Instances
  - No additional action needed, just recognizing a model based on some criteria



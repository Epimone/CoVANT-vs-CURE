# CoVANT-vs-CURE
<div align="center">

Technical Comparison of CoVANT and CURE

Three Research Questions, Mixed-Regime Evidence, and Component-Replacement Analysis

</div>

This repository page presents the complete figure set (Figures 1–5) and table set (Tables 1–15) from the technical-comparison paper. The figure captions, table captions, terminology, numerical values, analytical-estimate labels, and source references are retained from the paper.

Core execution pathsCURE: EMRC → HySAM → HySAM → LLF, with SIR operating as a parallel refinement and aggregation route.CoVANT: HMRC → HSpace (ESAIL, TSAIL, MACFuse) → SRR.

Contents

Figures 1–5

Tables 1–15

Figures 1–5

<a id="figure-1"></a>

Figure 1

<p align="center">
  <img src="./CoVANT_vs_CURE_abstract_overview.drawio%20%281%29%20%281%29.png" alt="Figure 1: Abstract-level distinction between CURE and CoVANT" width="100%">
</p>

Figure 1: Abstract-level distinction between CURE (left) and CoVANT (right). CURE is illustrated as a cascaded fusion system for heterogeneous unpaired modalities. HySAM performs an intermediate fusion in a hyperbolic–quantum representation space, after which LLF separately performs content-aware, mask-gated late fusion and handles missing modalities only after HySAM has completed its cross-fusion. CoVANT is illustrated for a combined regime in which a subject-aligned histopathology–multi-omics pair is integrated together with independently collected unpaired cytology, dermoscopy, and EHR modalities. CoVANT has no LLF. Instead, MeHyF implements dual intermediate fusion: HSpace/MACFuse first constructs a validity-constrained source set and then exchanges Euclidean and triplet-space evidence across modalities and spaces; unavailable sources are removed before attention normalization and context aggregation. This prevents absent evidence from entering the normalization denominator or contaminating the cross-fused context, and the resulting robust state is then carried through CoVANT’s combined paired-plus-unpaired pipeline. SRR subsequently performs a second intermediate aggregation across MeHyF layers. The two panels therefore differ in fusion timing, representation spaces, modality regime, and the location of missing-modality control. CoVANT source: p. 2, lines 49–79; p. 3, lines 91–110; App. F.1, p. 22, lines 756–781. CURE source: pp. 2–4; Fig. 3; Sec. 3.1; Algorithm 2.

<a id="figure-2"></a>

Figure 2

<p align="center">
  <img src="./%28a%29%20CoVANT_vs_CURE_1.drawio%20%281%29%20%281%29.png" alt="Figure 2(a): CoVANT architecture" width="100%">
</p>

<p align="center"><strong>(a) CoVANT: paired initial MeHyF processing followed by sequential updates with unpaired modalities.</strong></p>

<p align="center">
  <img src="./%28b%29%20CoVANT_vs_CURE_2.drawio%20%281%29%20%281%29.png" alt="Figure 2(b): CURE architecture" width="100%">
</p>

<p align="center"><strong>(b) CURE: sequential HyFuse stack with two HySAM operations, LLF, and SIR in each layer.</strong></p>

Figure 2: Detailed architecture-level distinction. In CoVANT (upper), HMRC first refines each input, one HSpace block produces two robust outputs through Euclidean/triplet-space processing and MACFuse, and the accumulated robust state is passed to the next MeHyF layer. Availability masks are used inside MACFuse before cross-modal/cross-space attention weights are normalized and before context is aggregated. SRR receives the already robust HSpace outputs and forms a separate second-stage intermediate-fusion path across layers. In CURE (lower), each HyFuse layer applies EMRC, then HySAM twice, and only afterwards passes the two HySAM outputs to LLF, where availability masks are first used explicitly. In parallel, SIR refines a HySAM-derived tensor. The final CURE representation concatenates the last LLF output with SIR outputs, whereas the final CoVANT representation concatenates the last HSpace robust output with channel-aligned SRR summaries. CoVANT source: p. 3, lines 91–110; pp. 5–7, lines 148–248; Eqs. (3), (11)–(16). CURE source: p. 3 Fig. 3; p. 4 Sec. 3.1; p. 6 Algorithm 2 and Eqs. (11)–(13); p. 7 Eqs. (14)–(15).

<a id="figure-3"></a>

Figure 3

<p align="center">
  <img src="./MeHyF_vs_HyFuse.drawio%20%281%29%20%281%29%20%281%29.png" alt="Figure 3: Layer-level comparison of MeHyF and HyFuse" width="100%">
</p>

Figure 3: Layer-level comparison. The upper row is CoVANT: (a) HMRC uses Ghost-based heterogeneous multi-scale branches; (b) HSpace constructs Euclidean and triplet-space representations and performs directed MACFuse exchange; and (c) SRR combines homogeneous and heterogeneous refinement. The lower row is CURE: (d) EMRC uses GPC, depthwise, and dilated-depthwise branches; (e) HySAM fuses Poincaré/Lorentz and quantum-inspired attention maps and is executed twice; (f) LLF is a separate late-fusion and missing-modality module; and (g) SIR is a compact heterogeneous four-branch refiner. HMRC and EMRC, HSpace and the HySAM–LLF pathway, and SRR and SIR implement different computations. CoVANT does not require a separate LLF because source validity is handled inside MACFuse before cross-modal/cross-space normalization. CoVANT source: p. 4, lines 117–145; pp. 5–7, lines 148–248; App. B.1 and App. D. CURE source: pp. 4–7, Secs. 3.1.1–3.1.4; Algorithm 2; App. B–C.

<a id="figure-4"></a>

Figure 4

<p align="center">
  <img src="./HSpace_vs_HySAM.drawio%20%282%29%20%281%29%20%281%29.png" alt="Figure 4: Mechanism-level comparison of HySAM and HSpace" width="100%">
</p>

Figure 4: Mechanism-level comparison of HySAM (upper) and HSpace (lower). CURE’s HySAM begins from GAP–DCT frequency descriptors. MHDGA computes Poincaré and Lorentz attention, MQIA forms complex quantum-inspired states and Born-rule amplitudes, LIL and MQIA mutually guide each other, and MAFG fuses the resulting attention maps. CURE does not compute signed-curvature token–anchor geodesic distances or Top-K supports. CoVANT’s HSpace instead begins with ESAIL, which selects Euclidean evidence and constructs context-conditioned atlas anchors; TSAIL learns signed curvature, selects Euclidean/spherical/hyperbolic geodesic branches, and retains Top-K token–anchor candidates; MTMix combines selected manifold context with Euclidean detail; HGA preserves pooled Euclidean context; and MACFuse performs directed, mask-aware cross-modal and cross-space exchange. CoVANT contains no Poincaré–Lorentz dual-attention map, quantum complex-state/Born-rule branch, or LIL–MQIA mutual guidance. CoVANT source: pp. 5–7, lines 153–235; Eqs. (4)–(14); Apps. C and E. CURE source: pp. 4–6; Figs. 4–5; Eqs. (1)–(10).

<a id="figure-5"></a>

Figure 5

<p align="center">
  <img src="./SRR_integral_components.drawio%20%284%29%20%281%29.png" alt="Figure 5: Refinement-level distinction between SRR and SIR" width="100%">
</p>

Figure 5: Refinement-level distinction. CoVANT’s SRR contains a Multi-Scale Hybrid Convolution Block with two explicit families. MHoC models homogeneous convolutional families through MDHC, MGHC, and MDDHC, each evaluated across multiple receptive fields and consolidated with channel shuffle and Ghost convolution. MHeC separately combines Ghost, depthwise, and dilated-depthwise convolutional paths. Their responses are combined residually. CURE’s SIR is a single compact heterogeneous four-branch block using GPC1×1, DWC3×3, DWC5×5, and DDC3×3, followed by channel shuffle, GPC, residual addition, and GELU. The branch taxonomy, convolutional building blocks, consolidation, and representational role are therefore different. CoVANT source: p. 7, lines 236–248; App. D, pp. 16–17. CURE source: p. 7 Sec. 3.1.4 and Eqs. (14)–(15); App. C.

Tables 1–15

<details>
<summary><strong>Table 1: Expanded response matrix for the four questions raised by the Scientific Integrity Chairs. The scientific criterion is stated generically; the detailed answer identifies the framework, module, experiment, figure, and table that satisfy or distinguish that criterion.</strong></summary>

Inquiry / sub-issue

Scientific assessment criterion

Detailed evidence-based answer

Question 1. What are the distinct research questions and principal contributions?





RQ1: cost-effective progressive representation learning

Expressive and cost-controlled progressive fusion requires modality-specific multi-scale evidence to be preserved, a shared state to be updated as heterogeneous modalities are incorporated, and model complexity to remain practical as the fusion pipeline grows.

CURE answers this question through HyFuse: EMRC is its principal efficiency mechanism, two successive HySAM calls provide Poincaré–Lorentz and quantum-inspired intermediate fusion, LLF performs post-HySAM late gating, and SIR refines HySAM-derived information. CoVANT also addresses effectiveness and efficiency, but through a different allocation: Ghost-based HMRC constructs inexpensive spatial evidence; HSpace jointly preserves Euclidean detail, estimates signed-curvature Euclidean/spherical/hyperbolic relations, selects Top-K supports, and performs validity-aware cross-modal/cross-space fusion; SRR supplies second-stage intermediate fusion. Under the common mixed-regime setup, full CoVANT-18 reaches 70.5/99.4/99.8/93.8/96.2 on BRCA/HAM10000/MORT versus 68.8/98.5/99.2/92.2/95.1 for CURE-18, while CURE remains less costly (7.71M/0.59 GFLOPs versus 14.6M/1.82 GFLOPs). Thus, the papers pursue different performance–cost frontiers through different components (see Figs. 1–3 and Tables 9, 10, and 11).

RQ2: unified paired-plus-unpaired learning

Unified mixed-regime learning requires one training/evaluation unit to preserve within-subject paired complementarity while incorporating cohort-independent unpaired modalities and optimizing their distinct tasks jointly, rather than demonstrating paired and unpaired capability in separate runs.

CURE focuses on either a grouped unpaired configuration or a paired configuration. CoVANT instead places paired BRCA WSI–omics, HAM10000, and MIMIC-III mortality in Group 1, and paired UCEC WSI–omics, SIPaKMeD, and MIMIC-III ICD in Group 2. The controlled comparison confirms that this is not only a dataset-label difference: after retraining CURE under CoVANT’s Group 1 setup, every matched CoVANT backbone is higher on BRCA, with gains of 1.71, 2.10, 2.73, and 3.52 C-index points for ResNet18, ResNet50, ViT-Tiny, and ShuffleNet, respectively; most HAM10000 and MORT metrics also improve (see Figs. 1–2 and Tables 8 and 9).

RQ3: effective and robust shared representation learning at practical cost

Effective and reliable shared representation learning requires complementary Euclidean local evidence and non-Euclidean hierarchical structure to be fused without allowing unavailable sources to enter cross-context formation, while retaining a favorable performance–cost balance across heterogeneous tasks.

CoVANT’s HSpace is the principal module that addresses effectiveness and missing-modality robustness jointly: ESAIL preserves Euclidean evidence and creates atlas anchors; TSAIL estimates signed-curvature Euclidean/spherical/hyperbolic geodesic relations and retains Top-K supports; MACFuse excludes unavailable modality–space sources before normalization and context aggregation. HSpace-only moves the CoVANT ablation baseline from 54.2 to 66.9 C-index, from 89.3/89.6 to 97.4/97.8 on HAM10000, and from 75.8/77.2 to 94.1/94.5 on MORT; the full HMRC–HSpace–SRR system reaches 70.5/99.4/99.8/93.8/96.2 at 14.6M/1.82 GFLOPs. CURE allocates the corresponding functions across separate stages: HySAM learns non-Euclidean/quantum-inspired representations, LLF handles missing modalities only after two HySAM calls, EMRC controls cost, and SIR refines a parallel route (see Figs. 3–4 and Tables 6, 10, and 11).

Principal contribution boundary

Contribution-level independence is established when each submission retains a complete research question, executable method, ablation program, and principal empirical claim after the other submission is treated as prior work.

CURE remains a complete cascaded hyperbolic–quantum intermediate–late fusion system. CoVANT remains a complete mixed-regime Euclidean/triplet-geometric dual-intermediate system with in-fusion source validity. Their representation objects, geometry, affinity equations, masking point, recurrent state, refiners, and evaluation units remain distinct (see Tables 3–7 and 14).

Question 2. Which methods, results, datasets, code, experiments, theory, and written material are shared?





Methods and equations

The relevant distinction is between shared standard primitives and reuse of a method-defining call graph, representation object, or equation sequence.

Both papers use ordinary operations such as convolution, pooling, residual addition, channel shuffle, softmax, Hadamard multiplication, and concatenation. Their method-defining computations differ: HMRC uses HCIL/Ghost branches whereas EMRC uses MHCF/GPC–DWC–DDC; HSpace uses ESAIL/TSAIL/MACFuse whereas CURE uses two HySAM calls followed by LLF; SRR uses homogeneous-plus-heterogeneous families whereas SIR uses one compact heterogeneous bank (see Fig. 3 and Tables 5, 6, and 7).

Datasets and tasks

Dataset overlap is assessed through subject alignment, modality composition, task, and unit of optimization rather than dataset name alone.

Twelve datasets overlap, but BRCA and UCEC have different roles: CoVANT uses subject-aligned WSI–omics pairs in its mixed groups, whereas CURE uses BRCA/UCEC omics in its unpaired grouping and evaluates paired cohorts separately. CoVANT additionally covers five paired WSI–omics cohorts and two combined groups; four datasets are unique to each paper (see Table 8 and the disclosure matrix).

Results and experiments

The comparison distinguishes repeated metric names from experiments that test the same hypothesis under the same modality alignment and training unit.

Both report ACC/AUC/C-index, efficiency, missing-modality, and order-related analyses. CoVANT uniquely evaluates mixed paired-plus-unpaired groups and signed-geometry/HSpace ablations. CURE uniquely evaluates HyFuse fusion-scheme, HySAM branch, LLF, and SIR analyses in its own regime-specific program. The new controlled tables now compare full models and module roles under the same CoVANT Group 1 setup (Tables 9–11).

Code and training infrastructure

General-purpose infrastructure can be shared without making the executed method-specific modules identical.

The general training utilities are infrastructure-level, whereas the method-specific module families are EMRC/MHCF, HySAM/MHDGA/PIL/LIL/MQIA/MAFG, LLF, and SIR for CURE, and HMRC/HCIL, ESAIL, TSAIL, Top-K, MTMix/HGA, MACFuse, and SRR/MHCB for CoVANT (see Table 13). The manuscripts do not establish file-level identity between these method-specific implementations.

Theory, prose, and figures

The comparison separates common terminology and formatting from any shared theorem, derivation, substantial prose, or model-specific visual evidence.

The papers do not present one shared theorem or one shared central equation chain. Figures 1–5 in this note expose different computational graphs, representation mechanisms, and refinement structures (see Table 13).

Question 3. Why would publication of either submission not render the other overly incremental?





Treat CURE as prior work

The counterfactual asks whether CoVANT retains a complete method and separately testable empirical claim after all CURE-specific ideas are treated as prior work.

CoVANT still contains ESAIL evidence selection and context-conditioned atlas anchors; signed-curvature Euclidean/spherical/hyperbolic token–anchor geodesics; Top-K support selection; MTMix/HGA; valid-source MACFuse before normalization; Ghost-based HMRC; homogeneous–heterogeneous SRR; five paired WSI–omics cohorts; and combined paired-plus-unpaired training. These mechanisms are visible in Figs. 2–5 and quantified by the full CoVANT and HSpace ablations (Tables 11 and 6). The controlled Group 1 comparison further shows that CoVANT-18 exceeds CURE-18 by 1.71 C-index and by 0.65–1.57 points on the four HAM10000/MORT metrics while testing the same mixed-regime unit (Table 9).

Treat CoVANT as prior work

The counterfactual asks whether CURE retains a complete method and separately testable empirical claim after all CoVANT-specific ideas are treated as prior work.

CURE still contains GAP–DCT frequency descriptors; Poincaré exponential mapping; Lorentz embedding and Minkowski modulation; quantum-inspired complex states and Born-rule amplitudes; LIL–MQIA mutual guidance; MAFG; two successive HySAM calls; exact-zero LLF; EMRC/MHCF; and compact heterogeneous SIR. CoVANT does not disclose this dual-hyperbolic–quantum frequency hypothesis or its intermediate–late schedule. CURE’s mixed-regime ablation still identifies HySAM as its principal effectiveness component and EMRC as its principal efficiency component (Figs. 3–4; Tables 10 and 14).

Controlled mixed-regime evidence

The controlled empirical comparison fixes the backbone, paired/unpaired group, task heads, data split, optimizer, schedule, and random seeds, leaving the complete fusion systems as the principal difference.

The supplied controlled comparison does this at the family level. CoVANT is higher on BRCA for all four matched backbones and is higher on most HAM10000/MORT metrics. The ResNet18 comparison is 70.52 versus 68.81 C-index, 99.38/99.80 versus 98.53/99.15 on HAM10000, and 93.81/96.20 versus 92.24/95.08 on MORT. The result supports non-incrementality because the CoVANT contribution remains empirically meaningful when CURE is implemented in CoVANT’s own mixed-regime setting (Table 9).

Module-responsibility evidence

Independent contributions are reflected in different module-level performance and cost allocations, not only in different names.

Under the same BRCA/HAM10000/MORT setup, HSpace adds 9.2 C-index points, 6.0/6.4 HAM points, and 12.7/14.5 MORT points when added to HMRC+SRR, whereas HySAM adds 6.0, 4.2/5.1, and 7.3/10.0 points when added to EMRC+LLF+SIR. HSpace therefore supplies a larger mixed-regime effectiveness contribution at higher cost, while EMRC makes CURE substantially lighter. These different responsibility profiles support distinct methods rather than equivalent implementations (Tables 10 and 11).

Question 4. Could the contributions reasonably be presented as a single paper?





Method nesting

A legitimate consolidation requires one method to be a nested component or small extension of the other, rather than a complete alternative with a different interface and objective.

The methods are not nested. HSpace consumes Euclidean spatial evidence, atlas anchors, signed-curvature geodesics, and valid modality–space sources; HySAM consumes GAP–DCT descriptors and produces Poincaré/Lorentz/quantum-informed attention maps, after which LLF performs a separate gate. One cannot remove a single branch and obtain the other (Figs. 3–4; Table 6).

One-for-one replacement is the interpretable test

An interpretable cross-family comparison preserves a single encoder, a single principal fusion system, and a single refiner so that each scientific choice remains attributable.

The meaningful experiment is direct replacement: HMRC ↔ EMRC, HSpace ↔ (HySAM+LLF), and SRR ↔ SIR under a fixed outer scaffold. Stacking both alternatives would not consolidate the papers; it would introduce competing modules and confounded pathways. The required replacement protocol is specified in Table 12.

Why the encoders cannot simply be combined

A controlled pre-fusion comparison uses one evidence-learning module for the subsequent fusion mechanism.

Serial or parallel EMRC+HMRC would apply two complete multi-scale convolutional learners to every modality. This would increase parameters/FLOPs, repeatedly transform the same modality-specific evidence, and make it impossible to determine whether gains arise from GPC/DWC/DDC processing or Ghost-enhanced HCIL processing. A one-for-one encoder replacement is scientifically interpretable; an EMRC+HMRC stack is a new unvalidated front end (Fig. 3(a,d); Tables 5 and 12).

Why the fusion cores cannot simply be combined

A coherent comparison uses one principal relation model and one recurrent state.

Stacking two HySAM calls together with HSpace would repeatedly refine the same pair using incompatible objects—frequency attention maps versus spatial query–anchor geometry—and would add LLF after MACFuse has already enforced source validity. This would increase cost, risk over-processing or attenuating modality-specific cues, distribute reliability control at different points, and leave the recurrent state ambiguous. Only replacing HySAM+LLF with HSpace, or HSpace with HySAM+LLF, isolates the scientific difference (Figs. 2–4; Tables 4, 6, and 12).

Why the refiners cannot simply be combined

A controlled refinement comparison uses one attributable summary family with fixed channel dimensions.

SIR and SRR are complete alternative refiners. Applying both would create multiple refinements of the same post-fusion evidence, enlarge the final concatenation, increase cost, and confound whether improvements arise from SIR’s compact heterogeneous branches or SRR’s homogeneous–heterogeneous MHCB. Direct SRR ↔ SIR replacement is the correct test (Fig. 5; Tables 7 and 12).

</details>

<details>
<summary><strong>Table 2: Three distinct research questions and the different methodological answers provided by CURE and CoVANT. Evidence is cited inside each answer so that the table is self-contained.</strong></summary>

ID

Research question

CURE: scope and methodological answer

CoVANT: scope and methodological answer

RQ1: cost-effective progressive representation learning

How can heterogeneous medical modalities be progressively integrated while preserving modality-specific evidence, learning expressive shared representations, and maintaining practical computational cost?

CURE directly formulates this objective. HyFuse uses EMRC for low-cost multi-scale encoding, two HySAM calls for Poincaré–Lorentz and quantum-inspired intermediate fusion, LLF for post-HySAM availability-aware late fusion, and SIR for refinement. Under the mixed-regime ablation, HySAM remains CURE’s principal effectiveness component and EMRC remains its principal efficiency component (Fig. 3; Table 10).

CoVANT addresses effectiveness and efficiency through another allocation. HMRC supplies Ghost-based multi-scale evidence; HSpace coordinates Euclidean detail, signed-curvature Euclidean/spherical/hyperbolic relations, Top-K supports, and validity-aware MACFuse; SRR supplies second-stage intermediate fusion. The full CoVANT-18 outperforms full CURE-18 on all five mixed-regime task metrics, while requiring a moderate additional budget (Tables 9 and 11).

RQ2: unified paired-plus-unpaired learning

Can one model jointly preserve subject-aligned paired WSI–omics complementarity and incorporate independently collected imaging/EHR modalities within the same grouped optimization and evaluation?

CURE focuses on either an unpaired grouped configuration or a paired configuration. Its architecture can be run in both regimes, but it does not formulate one training unit that jointly contains paired WSI–omics and independent unpaired modalities. When CURE is retrained under CoVANT’s combined Group 1 setup, it remains below the matched CoVANT variants, most clearly on paired BRCA (Table 9).

CoVANT directly instantiates the mixed unit: Group 1 jointly learns paired BRCA WSI–omics, HAM10000, and MIMIC-III mortality; Group 2 jointly learns paired UCEC WSI–omics, SIPaKMeD, and MIMIC-III ICD. The initial MeHyF layer preserves paired complementarity and later layers incorporate unpaired modalities into the same recurrent state (Figs. 1–2; Table 8).

RQ3: effective and robust shared representation learning at practical cost

How can robust shared representations exploit complementary Euclidean and non-Euclidean geometry, improve effectiveness across heterogeneous tasks, exclude unavailable sources before cross-context formation, and remain computationally practical?

CURE distributes these functions across modules. HySAM learns Poincaré/Lorentz and quantum-inspired frequency representations; LLF handles missing modalities after two HySAM calls; EMRC provides the major cost reduction; SIR refines the parallel route. This design is effective and highly economical, but missing-source control is not part of HySAM’s cross-fusion equations (Figs. 2–4; Table 10).

CoVANT concentrates effectiveness and robustness in HSpace. ESAIL preserves Euclidean evidence and creates anchors; TSAIL learns signed-curvature Euclidean/spherical/hyperbolic affinities and Top-K supports; MACFuse removes unavailable modality–space sources before normalization and context aggregation. HSpace contributes larger mixed-regime gains than HySAM in the matched ablations, while HMRC recovers a practical full-model budget (Tables 6 and 11).

</details>

<details>
<summary><strong>Table 3: Figure 1: system-level distinction in problem setting, fusion schedule, reliability control, and evaluation unit.</strong></summary>

System dimension

CoVANT

CURE

Why the distinction is substantive

Target learning condition

Explicitly supports paired, unpaired, and combined paired-plus-unpaired configurations. In the combined setting, a subject-aligned WSI–omics pair is processed together with independently collected imaging/EHR modalities.

It evaluates an unpaired grouped configuration or a paired configuration, but does not jointly optimize the two regimes within one grouped run.

The central empirical unit differs: CoVANT tests coexistence of paired complementarity and unpaired evidence in one grouped run.

Primary fusion paradigm

Parallel-to-sequential MeHyF with two intermediate-fusion stages: HSpace within each layer and SRR aggregation across layers.

Single-pass cascaded HyFuse with repeated HySAM intermediate fusion followed by a distinct LLF late-fusion stage; SIR forms a parallel refinement route.

The second CoVANT stage is inter-layer intermediate aggregation, whereas CURE explicitly inserts a late-fusion gate after repeated attention.

Initial modality processing

HMRC produces Ghost-enhanced, heterogeneous multi-scale spatial evidence tailored to ESAIL/TSAIL.

EMRC produces efficient multi-scale features through GPC, DWC, and DDC branches tailored to repeated HySAM processing.

The encoders use different branch families, consolidation layers, and downstream representational roles.

First representation/fusion stage

HSpace jointly constructs Euclidean evidence, atlas anchors, signed-curvature geodesic relations, sparse supports, and cross-modal/cross-space context.

HySAM constructs Poincaré/Lorentz and quantum-inspired frequency attention maps; MAFG fuses the attention streams, and HySAM is executed twice.

HSpace operates on spatial query–anchor relations and validity-constrained cross-space context; HySAM operates on frequency/channel attention maps and mutual hyperbolic–quantum guidance.

Second fusion/refinement stage

SRR aligns HSpace-derived robust features and aggregates them across MeHyF layers as the second intermediate-fusion stage.

LLF gates and concatenates the two HySAM outputs at late fusion; SIR separately refines one HySAM-derived path.

The final representations are assembled from different states and different secondary pathways.

Missing-modality intervention

MACFuse constructs a valid source modality–space set before attention normalization and context aggregation.

LLF introduces the explicit availability mask after the two HySAM calls and performs modality-level late gating.

CoVANT prevents unavailable sources from participating in cross-context formation; CURE suppresses them in the later fusion output.

Geometric scope

Preserved Euclidean detail plus bounded signed-curvature Euclidean, spherical, and hyperbolic query–anchor relations.

Negative-curvature Poincaré and Lorentz models plus a quantum-inspired complex-state/frequency branch.

The frameworks formulate different geometric objects and different affinity calculations.

Sparsification

TSAIL retains Top-K query–anchor supports and masks the remaining scores with −∞.

No token–anchor Top-K mechanism is defined in HySAM or LLF.

CoVANT explicitly selects a sparse geometric support set before fusion.

Recurrent state

The validity-filtered HSpace output $x_i^R$ is relabeled as the accumulated state for the next MeHyF layer.

The LLF output $x_i^{S'}$, produced after two HySAM calls, is propagated to the next HyFuse layer.

Availability affects CoVANT’s propagated state during HSpace fusion, but affects CURE’s propagated state only at LLF.

Final shared representation

Final HSpace robust state concatenated with SRR-aligned summaries from intermediate MeHyF layers.

Final LLF state concatenated with SIR-refined HySAM summaries.

The output families and their derivation paths are not interchangeable.

Principal empirical claim

One architecture supports mixed-regime learning, five paired WSI–omics cohorts, hybrid-geometry effectiveness, and missing-modality reliability.

One efficient cascaded architecture performs strongly when unpaired and paired regimes are evaluated through their respective configurations.

Publishing one does not answer the other’s principal learning question.

</details>

<details>
<summary><strong>Table 4: Figure 2: per-layer execution order, propagated states, and consequences for mixed-regime and missing-modality learning.</strong></summary>

Execution stage

CoVANT MeHyF

CURE HyFuse

Consequence for shared-state learning

Input organization

Initial combined-regime layer receives a subject-aligned WSI–omics pair; later layers receive the accumulated robust state and one independently collected modality.

Each layer receives the current modality and a preceding modality/shared state; the grouped unpaired setting is demonstrated with sequential independently collected modalities; paired runs are configured separately.

CoVANT’s initial shared state can encode within-patient paired complementarity before unpaired modalities are added.

Pre-fusion refinement

HMRC independently refines both inputs with two HCIL blocks and Ghost-based heterogeneous branches.

EMRC independently refines both inputs with MHCF blocks using GPC/DWC/DDC branches and pointwise consolidation.

The evidence supplied to the principal fusion module is generated by different convolutional systems.

Principal fusion invocation

One HSpace invocation coordinates ESAIL, TSAIL, HGA/MTMix, and MACFuse.

Two successive HySAM invocations coordinate MHDGA, MQIA, LIL–MQIA guidance, and MAFG.

CoVANT organizes one internally structured hybrid-geometry fusion; CURE repeats its attention mixer to enrich the feature pair.

Euclidean evidence path

ESAIL retains a spatial Euclidean detail path $x_j^E$ while constructing queries and atlas anchors.

HySAM begins from GAP–DCT descriptors; no separately preserved spatial Euclidean detail path is defined inside HySAM.

CoVANT explicitly carries local Euclidean evidence alongside manifold context.

Geometric relation path

TSAIL computes signed-curvature Euclidean/spherical/hyperbolic query–anchor scores and Top-K supports.

MHDGA computes Poincaré/Lorentz attention and MQIA supplies quantum-inspired guidance; no token–anchor support set is formed.

The recurrent states are informed by different relation structures.

Cross-modal exchange

MACFuse performs directed cross-modal and cross-space attention between valid source modality–space pairs.

MAFG combines hyperbolic and quantum-informed attention maps; the resulting features are passed through HySAM again.

CoVANT’s interaction graph is indexed by both modality and representation space.

Missing-source intervention

The valid source set $V_j^s$ is constructed before compatibility normalization; invalid sources receive zero influence and contribute no context.

LLF applies a modality mask to scalar logits after the two HySAM calls, then normalizes and concatenates weighted features.

CoVANT excludes invalid evidence during context formation; CURE removes it from the later fusion output.

State passed forward

Robust HSpace output $x_i^R$ after validity-aware fusion.

LLF output $x_i^{S'}$ after repeated HySAM and late gating.

The propagated state has undergone different reliability interventions.

Secondary path

SRR receives HSpace outputs, aligns channels, and aggregates them across layers as second-stage intermediate fusion.

SIR refines a HySAM-derived feature while LLF separately gates another HySAM-derived path.

CoVANT uses one robust-state family for recurrence and refinement; CURE retains separate LLF and SIR routes.

Final representation

Last HSpace output plus SRR-aligned intermediate summaries.

Last LLF output plus SIR-refined summaries.

The final concatenation combines different feature families and different fusion stages.

Within-paper empirical role

CoVANT Table 3 shows that HSpace supplies the largest single performance gain, while HMRC reduces the complete model’s cost and SRR adds complementary improvement.

CURE Table 3 shows that HySAM supplies the largest performance gain, EMRC supplies the largest efficiency gain, and LLF/SIR provide complementary reliability/refinement.

Each paper validates a different allocation of responsibilities across its modules.

</details>

<details>
<summary><strong>Table 5: Component-wise distinction corresponding to Fig. 3. The rows separate architectural construction, information role, fusion timing, and within-paper empirical responsibility.</strong></summary>

Module pair / sub-aspect

CoVANT

CURE

Substantive distinction and CoVANT role

HMRC versus EMRC: modality-specific evidence before fusion







Objective

HMRC is designed to preserve and diversify local spatial evidence before ESAIL/TSAIL by combining efficient Ghost-based heterogeneous transformations.

EMRC is designed to capture multi-scale modality-specific features at very low cost before two HySAM calls.

HMRC is specialized for the spatial query–anchor and Euclidean-detail requirements of HSpace; EMRC is specialized for the efficiency of repeated HySAM processing.

Internal block

Two HCIL blocks with heterogeneous branches {GC, DGSC, DGC} across multiple receptive fields.

Two MHCF blocks with branches {GPC, DWC, DDC} and pointwise/group-pointwise projections.

Different branch operators, block definitions, and feature-generation principles.

Consolidation

Channel shuffle and Ghost convolution generate additional responses through inexpensive transformations, followed by residual refinement.

Channel shuffle, GPC/PC, batch normalization, and GELU/ReLU consolidate the heterogeneous branches.

HMRC uses Ghost feature expansion; EMRC uses factorized pointwise/depthwise consolidation.

Empirical responsibility

CoVANT Table 3: HMRC alone improves all tasks while reducing the baseline from 23.6M/1.18 GFLOPs to 6.11M/0.71 GFLOPs; in the full model it offsets HSpace’s moderate cost.

CURE Table 3: EMRC is the principal efficiency driver; adding it to HySAM+LLF+SIR reduces 14.7M/1.45 GFLOPs to 7.71M/0.59 while improving performance.

Both are efficiency modules, but their implementations and downstream purposes are different.

HSpace including MACFuse versus HySAM followed by LLF: principal representation and reliability pathway







Composition

HSpace = ESAIL + TSAIL + MTMix/HGA + MACFuse.

HySAM = MHDGA + MQIA + MAFG with LIL–MQIA mutual guidance; LLF is an external subsequent module.

CoVANT integrates geometry learning, sparse support selection, cross-space fusion, and source validity inside one principal component.

Representation and affinity

Spatial Euclidean evidence and atlas anchors are related by signed-curvature Euclidean/spherical/hyperbolic geodesic scores and Top-K selection.

GAP–DCT descriptors drive Poincaré/Lorentz and quantum-inspired attention maps; no token–anchor geodesic support set is formed.

The systems use different representation units and different affinity formulations.

Cross-modal fusion

MACFuse performs directed exchange between different modalities and different spaces, with valid-source masking before normalization.

MAFG combines hyperbolic and quantum-guided attention maps; HySAM is applied twice, after which LLF performs modality-level late gating.

CoVANT jointly addresses predictive representation and in-fusion source reliability; CURE separates attention and missing-modality gating.

Empirical responsibility

CoVANT Table 3 identifies HSpace as the dominant single performance component; Table 4 validates its integration design; Table 12 validates the triplet geometry; missing-modality gains are attributed to HSpace/MACFuse.

CURE Table 3 identifies HySAM as the dominant performance component; the full HySAM exceeds its strongest MHDGA-only branch by 0.8–1.7 points, while LLF supplies exact-zero missing-modality gating.

Both are central, but the evidence supports different hypotheses and different reliability locations.

SRR versus SIR: refinement and cross-layer aggregation







Block taxonomy

SRR/MHCB separates MHoC and MHeC; MHoC contains MDHC, MGHC, and MDDHC families across multiple scales.

SIR uses one heterogeneous branch bank: GPC1×1, DWC3×3, DWC5×5, and DDC3×3.

SRR explicitly models homogeneous and heterogeneous convolution families; SIR uses a compact heterogeneous-only design.

Input and output role

SRR refines validity-filtered HSpace outputs, aligns channels across MeHyF layers, and supplies second-stage intermediate fusion.

SIR refines HySAM-derived features in a route parallel to LLF and contributes summaries to the final concatenation.

The refiners operate on different upstream states and serve different fusion topologies.

Empirical responsibility

CoVANT Table 3 shows SRR adds complementary gains to HMRC+HSpace and completes the dual-intermediate architecture.

CURE Table 3 shows SIR contributes smaller but systematic gains and stabilizes shared-feature learning.

SRR broadens cross-branch/cross-layer refinement; SIR prioritizes compact complementary refinement.

</details>

<details>
<summary><strong>Table 6: Detailed representation, geometric, interaction, and reliability distinction between CoVANT’s HSpace (including MACFuse) and CURE’s HySAM followed by the separate LLF module.</strong></summary>

Comparison item

CoVANT: HSpace = ESAIL + TSAIL + MACFuse

CURE: HySAM followed by LLF

Why the distinction matters / supporting evidence

Scientific objective

Jointly learn fine-grained Euclidean evidence, curvature-adaptive non-Euclidean relations, sparse reliable supports, cross-modal/cross-space context, and missing-source robustness within one principal fusion component.

Learn expressive Poincaré–Lorentz and quantum-inspired frequency attention through HySAM; address missing modalities afterwards through a separate lightweight LLF.

HSpace unifies effectiveness and reliability inside the fusion computation; CURE allocates these objectives to HySAM and LLF separately. CoVANT Sec. 2.2/Table 3; CURE Secs. 3.1.2–3.1.3/Table 3.

Input feature abstraction

ESAIL operates on refined spatial tensors $x'_j$, selects informative Euclidean evidence $\bar{x}_j$, and retains a local Euclidean path $x_j^E$.

HySAM summarizes refined tensors using $\psi_i=\operatorname{GAP}(\operatorname{DCT}(x'_i))$, yielding frequency/channel descriptors that drive attention maps.

CoVANT explicitly preserves spatial evidence while CURE builds its attention from pooled frequency descriptors.

Compact representation object

ESAIL tokenizes selected evidence into $M\ll HW$ context-conditioned atlas anchors $Z_j$ and forms query/key/value interfaces $(Q_j,\psi_j,\xi_j)$.

HySAM does not construct atlas anchors or query–anchor token pairs; it forms modality-specific Poincaré, Lorentz, and quantum-informed attention weights/maps.

The basic objects being compared and fused are different.

Euclidean pathway

Euclidean evidence is modeled explicitly through ESAIL, HGA, and the skip/detail path $x_j^E$; Euclidean geometry is also one branch of TSAIL.

Euclidean operations appear as standard preprocessing/norms, but HySAM’s proposed geometry-aware branches are Poincaré and Lorentz; it has no dedicated Euclidean evidence-to-anchor branch.

CoVANT is designed to preserve local Euclidean detail that may be underemphasized by a purely curved representation.

Non-Euclidean scope

TSAIL supports hyperbolic ($k<0$) and spherical ($k>0$) relations, together with a Euclidean branch within the near-zero branch $\lvert k\rvert\le\tau$.

MHDGA uses two negative-curvature hyperbolic models, Poincaré and Lorentz; MQIA adds a quantum-inspired complex Hilbert-state mechanism.

CoVANT uses positive-, zero-, and negative-curvature relation families; CURE uses dual hyperbolic models plus a non-geometric quantum-inspired branch.

Curvature parameterization

Bounded signed curvature $k=k_{\max}\tanh(\rho)$ and threshold masks select spherical, Euclidean, or hyperbolic scoring for each relation.

A positive magnitude $\tilde{c}$ parameterizes negative-curvature Poincaré/Lorentz mappings and is bounded for stability; there is no signed selector over three geometries.

CoVANT learns the geometry class used by token–anchor affinity; CURE adapts curvature within its dual-hyperbolic formulation.

Relation being scored

Each spatial query $Q_{u,j}$ is compared with each compact anchor $Z_{v,j}$, producing an explicit query–anchor relation.

Poincaré/Lorentz/quantum-informed attention is computed from modality frequency descriptors and transformed feature components rather than query–anchor pairs.

The relation graph is token–anchor in CoVANT and frequency/channel attention-map based in CURE.

Affinity calculation

TSAIL computes a squared geodesic distance $D^2_{uv,j}$ using the selected Euclidean/spherical/hyperbolic branch, converts it to $S_{uv,j}=-s_jD^2_{uv,j}$, and normalizes selected scores.

PIL uses a Poincaré exponential projection and Hadamard interaction; LIL uses Lorentz embedding and Minkowski modulation; MQIA uses complex states and Born-rule amplitudes; MAFG fuses their maps.

The equations implement different notions of compatibility, not alternative parameterizations of one score.

Sparse support selection

Only the leading $K_T$ anchors are retained for each query; all remaining scores are set to $-\infty$ before softmax.

HySAM and LLF define no corresponding Top-K token–anchor pruning step.

CoVANT imposes a sparse, context-adaptive support set. Table 12 separately validates the triplet geometry with ESAIL/MACFuse fixed.

Context mixing

MTMix aggregates selected atlas values, refines the geometry-aware context with a low-rank adaptor and HGA, and combines it with Euclidean detail and an input skip.

HySAM applies fused attention maps to the feature tensors; the two HySAM calls progressively enrich the same modality pair.

CoVANT combines sparse manifold context with an explicit Euclidean detail path; CURE repeatedly reweights features with hybrid attention maps.

Cross-modal and cross-space interaction

MACFuse realizes directed interactions $x^E_{i-1}\leftarrow x^T_i$, $x^T_{i-1}\leftarrow x^E_i$, and their reverse directions whenever the source modality is available.

MAFG fuses hyperbolic and quantum-guided attention maps using learnable modality weights; it does not define source–target attention over different modality–space pairs.

CoVANT’s interaction graph is indexed by both modality and representation space.

Fusion granularity

Gates and attention coefficients are defined over modality–space sources, and cross-context is computed separately for Euclidean and triplet target spaces.

HySAM forms modality-specific attention maps; LLF later computes one content-dependent scalar logit/weight per modality and concatenates weighted features.

CoVANT supplies finer-grained source-space control, whereas LLF is a lightweight modality-level late gate.

Missing-modality intervention point

Availability $m_\ell$ is used when constructing $V_j^s$ before compatibility normalization and context aggregation inside MACFuse.

Algorithm 2 executes HySAM at lines 6–7; availability first appears in LLF at line 8 and Eqs. (11)–(13).

The explicit mask changes CoVANT’s cross-fusion calculation itself, but changes CURE’s subsequent late-fusion output.

Normalization set under missingness

The softmax denominator contains only valid source modality–space pairs; invalid sources receive $\beta=0$. If the valid set is empty, the context is set to zero.

LLF modifies unavailable modality logits with a masking constant and normalizes the remaining modality-level gates; HySAM attention has already been computed.

CoVANT prevents unavailable sources from influencing normalization or context formation; CURE guarantees exact-zero contribution at late fusion.

Robust output and propagation

MACFuse produces robust HSpace outputs $x_j^R$ that are passed to SRR and, for the accumulated branch, to the next MeHyF layer.

LLF produces $x_i^{S'}$ after repeated HySAM; this state is passed to the next HyFuse layer, while SIR follows a separate path.

The recurrent shared state is created at different points relative to missing-source control.

Fusion schedule inside the layer

One HSpace block coordinates ESAIL/TSAIL and MACFuse; CoVANT further evaluates parallel versus cascaded ESAIL/TSAIL integration, with the parallel design improving by 0.8–2.3 points in Table 4.

Each HyFuse layer applies HySAM twice before LLF. CURE Table 3 shows the full HySAM exceeds the strongest MHDGA-only alternative by 0.8–1.7 points.

The papers validate different internal compositions and different repeated/parallel interaction hypotheses.

Primary within-paper performance role

HSpace alone moves CoVANT’s ablation baseline from 54.2 to 66.9 C-index on BRCA, 89.3/89.6 to 97.4/97.8 ACC/AUC on HAM10000, and 75.8/77.2 to 94.1/94.5 on MORT.

Removing HySAM from full CURE causes the largest drop, 2.4–9.9 points across tasks; LLF then adds the separate missing-modality late-fusion function.

Both papers identify their central attention/fusion pathway as the main effectiveness contributor, but the pathways solve different subproblems.

Missing-modality evidence

CoVANT reports 1.5–10.7 point gains across its paired missing-modality protocols and attributes the robustness to HSpace/MACFuse.

CURE evaluates omics-only, WSI-only, and missing-modality settings through LLF-based exact-zero gating on its paired cohorts.

The experiments support different reliability mechanisms; they are not direct same-setting evidence against each other.

Cost allocation

HSpace is the most computationally demanding CoVANT module (HSpace-only: 28.39M, 2.65 GFLOPs); HMRC reduces the complete HMRC+HSpace+SRR system to 14.6M and 1.82 GFLOPs.

CURE assigns the main efficiency burden to EMRC; adding EMRC to HySAM+LLF+SIR reduces 14.7M/1.45 GFLOPs to 7.71M/0.59.

CoVANT deliberately spends moderate cost on integrated geometry/reliability and recovers efficiency through HMRC; CURE uses a lighter HySAM–LLF pathway with EMRC as its efficiency driver.

Ablation program

CoVANT separately ablates HMRC/HSpace/SRR, ESAIL/TSAIL integration, attention replacements, and Euclidean/hyperbolic/spherical/triplet geodesics.

CURE separately ablates EMRC/HySAM/LLF/SIR, fusion-scheme replacements, HySAM attention replacements, and HySAM’s hyperbolic/quantum composition.

The independent ablation programs test different scientific hypotheses and help establish non-incrementality.

</details>

<details>
<summary><strong>Table 7: Detailed distinction between CoVANT’s Shared Representation Refiner (SRR) and CURE’s Shared Information Refinement (SIR).</strong></summary>

Refinement dimension

CoVANT SRR

CURE SIR

Architectural and representational consequence

Input state

Receives $x_j^R$, the robust HSpace output after Euclidean/triplet processing and validity-aware MACFuse.

Receives a HySAM-derived shared feature $x_i^S$ in a route parallel to LLF.

The refiners process states created by different representation and missing-modality mechanisms.

Primary objective

Align channels across MeHyF layers, enrich robust shared features, and implement the second intermediate-fusion stage.

Refine shared information, preserve salient multi-scale cues, and supply intermediate summaries that complement the final LLF output.

SRR is part of CoVANT’s dual-intermediate fusion; SIR complements CURE’s intermediate–late design.

Top-level block

Multi-Scale Hybrid Convolution Block (MHCB).

Compact heterogeneous multi-branch SIR block.

SRR explicitly combines two convolution families; SIR uses one heterogeneous family.

Homogeneous pathway

MHoC contains MDHC, MGHC, and MDDHC, each applied at 1 × 1, 3 × 3, and 5 × 5 scales.

No separately defined homogeneous depthwise/groupwise/dilated-depthwise family.

SRR models within-family scale variation before cross-family integration.

Heterogeneous pathway

MHeC combines Ghost, depthwise, and dilated-depthwise branches at different receptive fields.

One branch bank combines GPC1×1, DWC3×3, DWC5×5, and DDC3×3.

The heterogeneous operator sets and branch organization differ.

Cross-branch consolidation

Each homogeneous family is concatenated, channel-shuffled, and consolidated with Ghost convolution; MHoC and MHeC are then fused residually.

Branch outputs are concatenated, channel-shuffled, projected with GPC, and added residually before GELU.

SRR uses hierarchical family-level consolidation; SIR uses a single-stage compact consolidation.

Scale coverage

Multiple operator families are each evaluated over several receptive fields.

Four selected operator–scale combinations are evaluated once.

SRR explores a broader operator-by-scale grid, while SIR prioritizes compactness.

Output role

Produces channel-aligned $\hat{x}_j^R$ summaries that are aggregated with the final HSpace output.

Produces $\hat{x}_i^S$ summaries that are aggregated with the final LLF output.

Their outputs enter different final representations and originate from different upstream states.

Empirical role

CoVANT Table 3 shows SRR adds complementary gains to HMRC+HSpace; the full configuration outperforms variants without SRR while maintaining 14.6M/1.82 GFLOPs.

CURE Table 3 reports smaller but systematic drops (0.4–1.6 points) when SIR is removed.

Both refiners matter, but SRR’s broader structure supports CoVANT’s second intermediate-fusion objective.

Design boundary

SRR is broader and moderately more costly; its value depends on the already robust HSpace representation.

SIR is compact and efficient; it does not perform HSpace-style geometry selection or in-fusion validity control.

Their effects are architecture-dependent because the modules receive different feature states and serve different fusion schedules.

</details>

<details>
<summary><strong>Table 8: Evaluation-unit distinction: dataset role, subject alignment, task organization, and missing-modality testing.</strong></summary>

Evaluation dimension

CoVANT

CURE

Why this is a different empirical question

Paired WSI–omics coverage

Five cohorts: BRCA, UCEC, GBMLGG, KIRP, and BLCA.

Two WSI–omics cohorts: KIRP and BLCA; BraTS is an additional paired imaging setting.

CoVANT provides broader subject-aligned WSI–omics evidence.

Unpaired modality coverage

Independent imaging and EHR datasets; combined groups add these after the paired WSI–omics state.

Independent imaging, multi-omics, EHR, and time-series datasets in CURE’s unpaired grouped configuration.

Both include unpaired modalities, but they organize their grouped inputs differently.

Combined paired-plus-unpaired run

Group 1: paired BRCA WSI+omics + HAM10000 + MIMIC-III mortality. Group 2: paired UCEC WSI+omics + SIPaKMeD + MIMIC-III ICD. One group is trained at a time.

No evaluation inserts a paired WSI–omics cohort into the same grouped run as independent imaging and EHR modalities.

CoVANT tests retention of paired complementarity while unpaired evidence is progressively added.

BRCA role

In combined Group 1, BRCA is a subject-aligned WSI–omics pair and the task is survival analysis.

In CURE Table 1, BRCA is an unpaired multi-omics modality; no BRCA WSI is included in that grouped run.

The same cohort name denotes different input information and a different fusion problem.

UCEC role

In combined Group 2, UCEC is a paired WSI–omics input before SIPaKMeD and ICD9 are incorporated.

In CURE’s unpaired grouping, UCEC is represented by omics without paired WSI; in CoVANT Group 2, UCEC WSI and omics form the paired initial state.

The alignment structure and initial shared-state construction differ.

Initial fusion unit

Two subject-aligned modalities are processed in parallel by the first MeHyF layer.

The grouped unpaired sequence begins from independently collected modalities; CURE’s paired runs are configured separately.

CoVANT’s recurrent state can begin with within-patient paired complementarity.

Tasks in one grouped configuration

Group 1 jointly reports WSI–omics survival, dermoscopy classification, and mortality prediction; Group 2 reports WSI–omics survival, cytology classification, and diagnosis coding.

Unpaired tasks are grouped together; paired survival/imaging tasks are reported in dedicated evaluations.

CoVANT tests multiple alignment regimes and task types within one training/evaluation unit.

Missing-modality definition

Evaluated on paired WSI–omics cohorts using omics-only, WSI-only, and mixed single-modality test protocols after training with both modalities.

Evaluated on paired KIRP/BLCA settings through LLF using analogous missing-modality conditions.

Missing-modality testing is distinct from unpaired learning in both papers; the mechanisms used during fusion differ.

Availability mechanism linked to evaluation

HSpace/MACFuse removes unavailable source modality–space pairs before attention normalization and context formation.

LLF assigns zero modality weight after HySAM has produced the shared features.

The empirical robustness claim maps to different intervention points.

Principal empirical question

Can one model jointly exploit paired WSI–omics and independently collected unpaired modalities while supporting multiple tasks and remaining reliable under source absence?

Can one efficient cascaded HyFuse architecture remain effective when unpaired and paired regimes are evaluated in their respective configurations?

These claims are complementary rather than equivalent demonstrations.

CoVANT source: p. 8, lines 251–290; App. F.1, p. 22, lines 755–781. CURE source: p. 7 Sec. 4; Table 1 for the unpaired configuration; Table 2 for the paired and missing-modality configurations.

</details>

<details>
<summary><strong>Table 9: Controlled performance comparison of CURE and CoVANT families under the same combined paired-plus-unpaired configuration: paired BRCA WSI+omics, HAM10000, and MIMIC-III mortality. Values are mean±standard deviation over the retained runs.</strong></summary>

Model

Backbone

BRCA C-index

HAM ACC

HAM AUC

MORT ACC

MORT AUC

#P (M)

#F (G)

CURE-18

ResNet18

67.31±3.3

98.53±0.87

98.90±0.52

91.37±2.8

93.48±2.1

7.71

0.59

CoVANT-18

ResNet18

70.52±2.8

99.38±0.3

99.80±0.1

93.81±2.4

96.20±1.6

14.61

1.82

CURE-50

ResNet50

68.12±4.1

98.90±0.5

99.15±0.1

91.82±3.2

94.04±2.5

14.40

1.83

CoVANT-50

ResNet50

71.22±4.3

99.54±0.2

99.80±0.1

94.52±1.7

96.45±2.2

26.51

2.51

CURE-Ti

ViT-Tiny

68.12±2.8

99.08±0.1

99.08±0.0

92.30±2.6

94.80±3.4

10.80

1.25

CoVANT-Ti

ViT-Tiny

71.85±3.7

99.62±0.1

99.94±0.0

93.55±2.6

95.87±3.4

12.31

1.69

CURE-SN

ShuffleNet

63.70±2.2

94.35±1.4

94.80±0.9

87.95±1.8

88.41±1.2

3.10

0.29

CoVANT-SN

ShuffleNet

68.22±1.3

97.84±0.5

98.13±0.2

90.37±1.6

93.64±2.6

5.21

1.14

</details>

<details>
<summary><strong>Table 10: Module-wise ablation of CURE-18 under the CoVANT combined paired-plus-unpaired setup.</strong></summary>

EMRC

HySAM

LLF

SIR

BRCA C-index

HAM10000 ACC

HAM10000 AUC

MORT ACC

MORT AUC

#P

#F

×

×

×

×

54.8

89.3

89.3

74.5

76.9

11.2

0.59

✓

×

×

×

58.4

92.7

92.8

78.1

78.5

5.53

0.38

×

✓

×

×

63.7

96.5

96.5

86.2

87.1

13.7

1.12

×

×

✓

×

57.1

91.7

91.7

79.4

79.3

12.2

0.65

×

×

×

✓

59.9

93.5

93.5

81.7

81.9

13.7

0.89

✓

×

✓

✓

62.8

94.3

94.1

84.9

85.1

6.82

0.47

✓

✓

✓

×

65.8

97.7

97.9

90.2

91.9

7.53

0.55

×

✓

✓

✓

66.5

98.1

98.3

90.7

92.4

14.7

1.45

✓

✓

✓

✓

67.3

98.5

98.9

91.4

93.5

7.71

0.59

</details>

<details>
<summary><strong>Table 11: Module-wise ablation of CoVANT-18 under the same combined paired-plus-unpaired setup.</strong></summary>

HMRC

HSpace

SRR

BRCA C-index

HAM10000 ACC

HAM10000 AUC

MORT ACC

MORT AUC

#P

#F

×

×

×

54.2

89.3

89.6

75.8

77.2

23.6

1.18

✓

×

×

56.4

91.3

91.6

78.4

79.6

6.11

0.71

×

✓

×

66.9

97.4

97.8

94.1

94.5

28.39

2.65

×

×

✓

57.9

91.9

92.1

79.3

79.7

23.71

1.21

✓

×

✓

61.3

93.4

93.4

81.1

81.7

10.7

0.89

✓

✓

×

69.7

98.9

99.2

92.5

95.7

14.5

1.79

×

✓

✓

69.9

99.1

99.4

93.1

95.7

32.2

3.34

✓

✓

✓

70.5

99.4

99.8

93.8

96.2

14.6

1.82

</details>

<details>
<summary><strong>Table 12: One-for-one component-replacement ablation under the combined paired-plus-unpaired Group 1 configuration. The CURE-18 and CoVANT-18 rows are measured. The replacement rows are analytical estimates derived from the measured full-model and module-wise ablations in Tables 10–11.</strong></summary>

Fixed scaffold

Component configuration

BRCA C-index

HAM10000 ACC

HAM10000 AUC

MORT ACC

MORT AUC

#P (M)

#F (G)

Interpretation

CURE-18

EMRC + HySAM + LLF + SIR

67.31

98.53

98.90

91.37

93.48

7.71

0.59

Native CURE reference under the CoVANT Group 1 mixed-regime setup.

CoVANT-18

HMRC + HSpace + SRR

70.52

99.38

99.80

93.81

96.20

14.61

1.82

Native CoVANT reference under the same setup.

CURE

HMRC + HySAM + LLF + SIR

66.8

98.3

98.8

90.9

93.0

8.29

0.92

HMRC preserves CURE’s fusion sequence, but its Ghost-enhanced evidence is designed for ESAIL–TSAIL rather than repeated HySAM processing; therefore, a small effectiveness reduction and a moderate cost increase are expected.

CoVANT

EMRC + HSpace + SRR

69.9

99.0

99.5

93.2

95.8

14.03

1.49

EMRC lowers the CoVANT front-end cost, but its GPC/DWC/DDC evidence is less specialized for the spatial query–anchor and Euclidean-detail requirements of HSpace than HMRC’s Ghost-enhanced evidence.

CURE

EMRC + HSpace + SIR

68.5

99.2

99.5

92.1

94.3

9.62

1.35

Replacing HySAM+LLF with HSpace is expected to improve mixed-regime learning because Euclidean evidence, signed-curvature triplet geometry, Top-K support selection, and source-validity control are integrated before cross-context formation.

CoVANT

HMRC + HySAM + LLF + SRR

68.7

98.4

98.5

93.2

95.1

12.70

1.06

Replacing HSpace with HySAM+LLF removes Euclidean atlas anchors, signed Euclidean/spherical/hyperbolic token–anchor scoring, Top-K selection, and in-fusion source-validity control. The resulting model remains effective but is expected to lose part of CoVANT’s mixed-regime advantage.

CURE

EMRC + HySAM + LLF + SRR

67.0

98.4

98.8

91.1

93.2

7.64

0.58

SRR introduces broader homogeneous–heterogeneous refinement, but its second-stage intermediate-fusion design is not fully aligned with the recurrent HySAM–LLF state used by CURE; therefore, only limited transfer is expected.

CoVANT

HMRC + HSpace + SIR

70.0

99.2

99.5

93.3

95.9

14.68

1.83

SIR retains a useful heterogeneous refinement pathway, but it does not reproduce SRR’s explicit homogeneous–heterogeneous refinement or its second-stage intermediate fusion across HSpace outputs.

</details>

<details>
<summary><strong>Table 13: Shared-material and method-boundary matrix. The table reports the overlap established from the manuscripts and the corresponding method-specific distinction.</strong></summary>

Category

Shared or overlapping material established from the manuscripts

Method-defining distinction and interpretation

Research scope

Both works study representation learning from heterogeneous medical data, shared-representation learning, multi-task prediction, efficiency, and robustness.

CURE tests cascaded hyperbolic–quantum intermediate–late fusion; CoVANT tests validity-aware Euclidean/spherical/hyperbolic token–anchor fusion for paired-plus-unpaired learning (Figs. 1–4; Tables 2 and 6).

High-level data flow

Both progressively carry a learned state while incorporating later modalities and aggregate intermediate information for prediction.

CURE propagates the post-HySAM LLF output and separately aggregates SIR features; CoVANT propagates the validity-filtered HSpace output and separately aggregates SRR features (Figs. 2–3; Table 4).

Methods

Both employ standard primitives, including convolution, residual addition, pooling, channel shuffle, softmax, Hadamard multiplication, concatenation, and learnable weights.

The method-defining call graphs differ: HMRC uses HCIL/Ghost branches whereas EMRC uses MHCF/GPC–DWC–DDC; HSpace uses ESAIL/TSAIL/MACFuse whereas CURE uses repeated HySAM followed by LLF; SRR uses MHoC+MHeC whereas SIR uses one compact heterogeneous branch bank (Fig. 3; Tables 5–7).

Technical results

Both report predictive performance, efficiency, missing-modality, and order-related analyses on several common benchmarks, and several baseline families recur.

CoVANT evaluates combined paired-plus-unpaired groups, five paired WSI–omics cohorts, HSpace integration, signed-geometry choices, and MACFuse-centered missingness. CURE evaluates fusion schemes, HyFuse modules, HySAM branches, LLF, and regime-specific unpaired/paired/missing-modality settings. The controlled Group 1 results and both mixed-regime ablations are reported in Tables 9–11.

Datasets

Twelve datasets overlap: BRCA, UCEC, GBMLGG, KIRP, BLCA, HAM10000, SIPaKMeD, PathMNIST, OrganAMNIST, MIMIC-III, MHEALTH, and UCI-HAR.

Four datasets are unique to each work, and shared TCGA cohorts can have different roles. In CoVANT Group 1, BRCA is a subject-aligned WSI–omics pair; in CURE’s grouped unpaired configuration, BRCA contributes omics without BRCA WSI (Table 8).

Experimental configurations

Both use heterogeneous medical tasks and common metrics such as ACC, AUC, C-index, parameters, and FLOPs.

CURE evaluates either an unpaired configuration or a paired configuration in a given experiment. CoVANT additionally defines a single grouped unit containing a paired WSI–omics cohort and independent unpaired imaging and EHR modalities. Under that common unit, the CoVANT family is higher on BRCA for every matched backbone and on most HAM10000/MORT metrics (Tables 8 and 9).

Backbones, baselines, and metrics

ResNet18/50, ViT-Tiny, ShuffleNet, several multimodal baselines, and the principal predictive and complexity metrics overlap.

The complete baseline sets, model variants, paired-cohort coverage, and method-specific ablation targets differ across the two works.

Training infrastructure

Both report 200 epochs, Adam with initial learning rate $10^{-3}$, ReduceLROnPlateau, an A100 80GB environment, five random seeds, and 128 × 128 preprocessing where applicable.

These shared settings are experimental infrastructure rather than the proposed contribution; the task grouping, modality alignment, method-specific modules, and several datasets remain different.

Code

The manuscripts link separate repositories and do not provide a file-level cross-repository comparison. Generic training utilities may be reusable across the research program.

CURE’s method-specific components are EMRC/MHCF, MHDGA/PIL/LIL/MQIA/MAFG/HySAM, LLF, and SIR. CoVANT’s are HMRC/HCIL, ESAIL, TSAIL, Top-K, MTMix/HGA, MACFuse, and SRR/MHCB. The manuscript-level comparison establishes distinct method-specific call graphs; it does not assign file-level identity to general-purpose utilities.

Theoretical arguments

Both motivate geometry-aware representation learning for heterogeneous structure; neither presents a shared theorem or proof.

CURE derives Poincaré/Lorentz frequency attention, Minkowski modulation, quantum-inspired complex states/Born-rule amplitudes, and MAFG. CoVANT derives Euclidean evidence-to-anchor construction, signed-curvature Euclidean/spherical/hyperbolic token–anchor distance, Top-K support selection, and validity-aware cross-space attention (Fig. 4; Table 6).

Written material

Standard domain definitions, dataset descriptions, metric descriptions, and training terminology can recur because the papers address the same application domain.

The central method descriptions, equations, module names, and principal claims correspond to different computational systems. The method descriptions, equations, module names, and principal claims correspond to different computational systems.

Figures and qualitative protocol

Some figures use the same broad modality types, and qualitative analyses may use common input samples or visualization protocols for comparability.

Figures 1–5 in this note show different architecture, module, and mechanism graphs. The qualitative sections are presented as model-specific evidence associated with the corresponding CURE and CoVANT systems.

</details>

<details>
<summary><strong>Table 14: Counterfactual contribution test with method-level and empirical evidence embedded in each answer.</strong></summary>

Prior-work assumption

Independent contribution that remains

Why the remainder is substantial and separately publishable

CURE is treated as prior work

CoVANT retains HMRC/HCIL; ESAIL evidence selection and context-conditioned atlas anchors; signed-curvature Euclidean/spherical/hyperbolic query–anchor geodesics; Top-K support selection; MTMix/HGA; valid-source MACFuse before normalization; homogeneous–heterogeneous SRR; five paired WSI–omics cohorts; and two combined paired-plus-unpaired groups.

CURE does not disclose CoVANT’s representation unit, signed three-geometry selector, sparse query–anchor support set, modality–space validity graph, or mixed-regime training unit (Figs. 2–5; Tables 6 and 8). Under the exact CoVANT Group 1 setup, CoVANT-18 remains 1.71 C-index points higher on BRCA and 0.65–1.57 points higher on HAM10000/MORT metrics than CURE-18; the BRCA gain is 1.71–3.52 points across matched backbones (Table 9). HSpace also adds larger mixed-regime gains than HySAM in the respective leave-one-path-out ablations (Tables 10 and 11).

CoVANT is treated as prior work

CURE retains EMRC/MHCF; GAP–DCT frequency descriptors; Poincaré exponential mapping; Lorentz embedding and Minkowski modulation; quantum-inspired complex states and Born-rule amplitudes; LIL–MQIA mutual guidance; MAFG; two successive HySAM calls; exact-zero LLF; and compact heterogeneous SIR.

CoVANT does not disclose CURE’s dual-hyperbolic plus quantum-inspired frequency hypothesis, mutual-guidance equations, repeated HySAM schedule, or separate intermediate–late fusion design (Figs. 3–4; Table 6). CURE’s ablation remains internally complete under the mixed regime: HySAM supplies the largest effectiveness gain, EMRC provides the main reduction from 14.7M/1.45G to 7.71M/0.59G, and LLF/SIR provide separate reliability/refinement roles (Table 10). Thus, CoVANT does not anticipate CURE’s central mathematical contribution.

Shared datasets and infrastructure are treated as non-novel

The complete central call graphs remain HMRC→HSpace→SRR and EMRC→HySAM→HySAM→LLF with SIR.

Removing common datasets, backbones, metrics, optimizer settings, residual operations, pooling, and concatenation does not collapse either method into the other. The remaining differences concern the scientific mechanisms themselves (Tables 5–7 and 13).

All common benchmark results are treated as prior context

CoVANT retains its mixed-regime family comparison, signed-geometry/HSpace analyses, and in-fusion missing-source tests; CURE retains its HyFuse, HySAM, LLF, EMRC, and SIR analyses.

The experiments answer different causal questions. CoVANT tests which Euclidean/non-Euclidean and validity-aware mechanisms make one combined paired-plus-unpaired run effective; CURE tests which hyperbolic–quantum, late-gating, and efficiency components make its cascaded regime-specific design effective. The independent ablation programs are summarized in Tables 10, 11, and 12.

</details>

<details>
<summary><strong>Table 15: Why direct component stacking is not a valid consolidation, and why one-for-one replacement is the scientifically interpretable comparison.</strong></summary>

Functional stage

CURE component

CoVANT component

Why combination is not a coherent single-paper method; valid comparison

Pre-fusion convolutional learning

EMRC uses MHCF blocks with GPC, DWC, and DDC operations and is CURE’s main efficiency mechanism.

HMRC uses HCIL blocks with Ghost, DGSC, and DGC operations and prepares spatial evidence for HSpace.

Using EMRC and HMRC together would place two complete multi-scale convolutional learners before every fusion stage. This would increase parameters/FLOPs, repeatedly transform the same modality-specific evidence, and prevent attribution of gains to either GPC/DWC/DDC or Ghost-enhanced HCIL processing. The valid test is EMRC↔HMRC replacement with the rest of the scaffold fixed (Fig. 3(a,d); Tables 5 and 12).

Principal representation and fusion learning

Two HySAM calls learn Poincaré/Lorentz and quantum-inspired frequency attention; LLF subsequently performs scalar mask-aware late fusion.

HSpace learns Euclidean evidence, signed-curvature Euclidean/spherical/hyperbolic query–anchor relations, Top-K supports, and validity-aware MACFuse.

Applying HySAM twice and then HSpace, or HSpace and then HySAM twice, would repeatedly refine the same pair using incompatible feature objects–frequency attention maps versus spatial query–anchor geometry. It would also combine MACFuse’s pre-normalization source exclusion with LLF’s post-fusion scalar gate, distribute reliability control, increase cost, risk attenuation of modality-specific cues, and leave the recurrent state undefined. The valid test is HSpace↔(HySAM+LLF) replacement (Figs. 2–4; Tables 4, 6, and 12).

Cross-layer refinement

SIR is a compact heterogeneous four-branch refiner whose outputs are concatenated with the final LLF route.

SRR combines homogeneous and heterogeneous families and implements CoVANT’s second intermediate-fusion stage over HSpace outputs.

Applying both refiners would create multiple transformations of the same post-fusion evidence, enlarge the final concatenation, increase cost, and confound whether any gain comes from SIR’s compact heterogeneous bank or SRR’s MHoC+MHeC design. The valid test is SIR↔SRR replacement (Fig. 5; Tables 7 and 12).

Evaluation unit

CURE is organized around either an unpaired grouped evaluation or paired evaluations.

CoVANT additionally optimizes paired WSI–omics and independent unpaired modalities in one mixed group.

A merged method would need a new factorial program spanning both outer scaffolds, all encoder/fusion/refiner replacements, and paired, unpaired, and mixed regimes under matched budgets. This is not editorial consolidation; it is a new cross-family selection study (Tables 8 and 9).

Central scientific question

When does cascaded hyperbolic–quantum intermediate fusion followed by late gating provide the best performance–efficiency trade-off?

When does validity-aware Euclidean/triplet-geometric dual-intermediate fusion provide effective and robust mixed-regime learning?

Combining the papers would require a new question: how should alternative convolutional learners, geometry systems, masking locations, and refiners be selected or replaced across modality-alignment regimes? Neither current paper claims or evaluates that selection problem. Each existing paper is complete without the other, so separate presentation is the coherent scientific organization.

</details>

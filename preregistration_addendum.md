# Addendum to the preregistration

Andrew Gibson  
November 11, 2026

My preregistered test in the Michaelov et al. data was not significant (arousal -0.096µV per SD, p=0.417). That dataset had only about a 15% chance of detecting an effect of this size so I am adding a test in open data I have not analyzed.

**Data**
I will pool 4 sets of words from 3 datasets:

1. The final word of each sentence in Szewczyk and Federmeier [1] (https://osf.io/urvax).
2. The final noun of each sentence in Nieuwland et al. [2] (https://osf.io/eyzaq).
3. The content words before the article in the same Nieuwland et al. sentences which I will take from their raw EEG.
4. The content words in DERCo [3] (https://osf.io/rkqbu) where 20 people read five short stories one word at a time.

As in my pilot, I leave out the first and last word of each sentence in sets 3 and 4. For these two sets, the N400 is the mean voltage from 300 to 500ms at centro-parietal electrodes. I will make processing choices before relating the N400 to emotion. If I cannot process one of the datasets, I will pool the rest.

**What I have seen:** The words and their ratings, how well language models predict them and, for a power analysis, how noisy the N400 is in models without the emotion ratings or with them shuffled across words. I have not yet related the N400 to emotion in these data.

**Model:** The original model with three changes. Surprisal is raised to the power 0.6, which better predicts the N400 [4] and comes from the open language model that best predicts each dataset's words. Frequency and orthographic neighbors are computed the same way for every dataset. Word position and, where available, the baseline, study, lab and presentation format are added as controls. Readers are assigned their own intercept and arousal slope. Items and words have their own intercepts with the same fallback as before.

**Test:** I will fit the model to each set and pool the arousal estimates and weigh each by its precision. **As in my original plan, I will count my pilot as replicated if the pooled estimate is negative with p<0.05.** Given the effect size in my earlier data, this test has roughly a 70 to 95% chance of detecting said effect. It is possible that the test will not be significant. In this case, I will test whether the pooled estimate is closer to zero than -0.073µV per SD (one-sided p<0.05). In other words, the effect my pilot had only a 33% chance of detecting [5]. If it is, my pilot was too small to detect an effect this size and its result was most likely a false positive. I'm keeping valence and extremity in the model but not making any predictions about them.

[1] Szewczyk, J. M., & Federmeier, K. D. (2022). Context-based facilitation of semantic access follows both logarithmic and linear functions of stimulus probability. *Journal of Memory and Language, 123*, 104311.  
[2] Nieuwland, M. S., et al. (2018). Large-scale replication study reveals a limit on probabilistic prediction in language comprehension. *eLife, 7*, e33468.  
[3] Quach, B. M., Gurrin, C., & Healy, G. (2024). DERCo: A dataset for human behaviour in reading comprehension using EEG. *Scientific Data, 11*, 1104.  
[4] Michaelov, J. A., & Bergen, B. K. (2024). On the mathematical relationship between contextual probability and N400 amplitude. *Open Mind, 8*, 859-897.  
[5] Simonsohn, U. (2015). Small telescopes: Detectability and the evaluation of replication results. *Psychological Science, 26*(5), 559-569.

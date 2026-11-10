# Does word emotion predict readers' N400s after controlling for language model surprisal?

Preregistration of a confirmatory test  
Andrew Gibson  
November 10, 2026

## Background

About 400ms after a word appears, the brain produces a negative voltage called the N400. The less expected a word is, the larger, i.e. more negative, this potential becomes [1]. In a 2024 study, Michaelov and colleagues found that surprisal from language models (LMs), or how unexpected a word is given its context, explains N400 differences once attributed to how much a word made sense in context [2]. However, LMs learn from text alone and it is not yet known whether the emotion content of a word changes the N400 more than what an LM predicts. In an exploratory pilot using a separate open dataset [3], more arousing words produced larger N400s than surprisal predicted. Here I test whether this holds in new participants and for new sentences.

## Hypothesis

Three outcomes are possible. If the N400 only tracks predictability, arousal should not matter once surprisal is accounted for (H0). Otherwise, more arousing words should produce either smaller N400s (H1) or larger N400s (H2). **Based on my pilot, I hypothesize that more arousing words produce larger N400s with LM surprisal and word-level controls held constant (H2).**

## Data

I will use the open single trial EEG data from Michaelov et al. [2] (https://osf.io/pysbc). Fifty participants read sentences one word at a time and each sentence frame ended in one of four ways. There are 500 sentence-final words in total. For each trial, the authors provide the N400 to the final word at nine centro-parietal electrodes, with mean voltage being 300 to 500ms after the word appears and are baseline corrected. They also provide GPT-3 surprisal, word frequency and orthographic neighborhood size for each word. The sample size is fixed because these data already exist.

## What I have seen so far

I've read the original paper and the author's analysis code. Before writing this, I looked at the list of final words and their emotion ratings and checked that arousal is similar across the four sentence endings. I also checked the number of participants, trials and electrodes, and the overall mean and spread of the N400. I have not run any analysis that relates the N400 to emotion, or to any other word property, in this dataset. My pilot used different data [3].

## Variables

- **N400 is the outcome:** The average over the nine electrodes in µV on each trial. Lower values are larger N400s.
- **Arousal is the key predictor:** Ratings from Warriner et al., 2013 [4] from calm=1 to excited=9.
- **Valence and extremity:** Valence goes from unhappy=1 to happy=9. Extremity is how far valence is from the neutral midpoint of 5. As in my pilot, both go into the model along with arousal but I make no predictions about them.
- **Surprisal:** GPT-3 davinci surprisal in bits as provided by the authors [2].
- **Word-level controls:** Word frequency and orthographic neighborhood size (Coltheart's N) as provided by the authors [2], word length in letters, and concreteness ratings [5].

I match each word to its ratings by exact spelling first. If there is no exact match, I use its base form (e.g., said to say, glasses to glass) with the same script as my pilot. I will check every base-form match by hand before running an N400 analysis. After exclusions, all predictors are z-scored across trials so effects are in µV per SD.

## Exclusions

I will exclude trials whose final word has no emotion or concreteness rating after matching. I will not exclude any participants or trials beyond the artifact rejection the authors already did [2] and I will not remove outliers.

## Analysis

I will fit a linear mixed-effects model in R with lme4 [6] by maximum likelihood:

```r
m1 <- lmer(n400 ~ surprisal + frequency + neighborhood + length + concreteness +
             valence + extremity + arousal +
             (1 + arousal | subject) + (1 | frame) + (1 | word),
           data = d, REML = FALSE, control = lmerControl(optimizer = "bobyqa"))
m0 <- update(m1, . ~ . - arousal)
anova(m0, m1)
```

The random effects procedure is the same as that of the original authors [2] with two changes. Because I average the electrodes first, I do not need their random intercept for electrode. I will also add a by subject random slope for arousal given it is the effect being tested [7]. The test is the likelihood-ratio χ²(1) between m0 and m1, which differ only in the fixed effect of arousal. It is possible that m1 will not converge or will give a singular fit. In this case I will drop the arousal slope which leaves the author's own random effects structure.

**I will count my pilot as replicated if p<0.05 and the arousal estimate is negative**, i.e. larger N400s for more arousing words. A significant positive estimate would support H1 instead and p>=0.05 would mean this dataset gives no evidence for an arousal effect beyond surprisal. I will not correct this test for multiple comparisons because this is a single, focused test. I will report the estimate and its 95% confidence interval regardless of the outcome.

## Secondary question

I will also test whether the arousal effect depends on how predictable the word is. To do this, I will add an arousal x surprisal term to the final model above and test it the same way (χ²(1)). Accounts of the N400 disagree on which direction this should go, so I make no prediction.

## Everything else

Other analyses of these data are exploratory with p-values corrected by the Benjamini-Hochberg false discovery rate.

## References

[1] Kutas, M., & Federmeier, K. D. (2011). Thirty years and counting: Finding meaning in the N400 component of the event-related brain potential (ERP). *Annual Review of Psychology, 62*, 621-647.  
[2] Michaelov, J. A., Bardolph, M. D., Van Petten, C. K., Bergen, B. K., & Coulson, S. (2024). Strong prediction: Language model surprisal explains multiple N400 effects. *Neurobiology of Language, 5*(1), 107-135.  
[3] Frank, S. L., Otten, L. J., Galli, G., & Vigliocco, G. (2015). The ERP response to the amount of information conveyed by words in sentences. *Brain and Language, 140*, 1-11.  
[4] Warriner, A. B., Kuperman, V., & Brysbaert, M. (2013). Norms of valence, arousal, and dominance for 13,915 English lemmas. *Behavior Research Methods, 45*(4), 1191-1207.  
[5] Brysbaert, M., Warriner, A. B., & Kuperman, V. (2014). Concreteness ratings for 40 thousand generally known English word lemmas. *Behavior Research Methods, 46*(3), 904-911.  
[6] Bates, D., Mächler, M., Bolker, B., & Walker, S. (2015). Fitting linear mixed-effects models using lme4. *Journal of Statistical Software, 67*(1), 1-48.  
[7] Barr, D. J., Levy, R., Scheepers, C., & Tily, H. J. (2013). Random effects structure for confirmatory hypothesis testing: Keep it maximal. *Journal of Memory and Language, 68*(3), 255-278.

<projtitle>Interactions between motor behavior & episodic memory</projtitle>

<div style="display: flex; justify-content: left; align-items: top; flex-wrap: wrap; gap: 40px; padding-top: 15px; padding-bottom:15px">

  <div>
    <b>quick links:</b>
  </div>
  
  <div>
    <a href="https://github.com/camillegasser/psychsci_scaffold" target="_blank"><b>GitHub repository</b></a>
    <br>
    (analysis code)
  </div>
  
  <div>
    <a href="https://osf.io/xgwzf/" target="_blank"><b>OSF page</b></a>
    <br>
    (all study materials)
  </div>
  
  <div>
    <a href="/docs/GasserDavachi_2023.pdf" target="_blank"><b>research paper</b></a>
    <br>
    (published in <i>Psych Science</i>)
  </div>
</div>

<hr>
<h4>Motivation</h4>

Our lives are full of familiar & repetitive behaviors: commuting to work, getting ready for bed, typing out an email. But these routines don't exist in a vacuum. Alongside our actions, we face a constant influx of sensory experiences, many of which might be useful to store in long-term memory.

Despite the collision of learning and behavior in everyday life, almost all research on memory treats motor actions as little more than a confound — something included in experiments just to keep subjects from getting too bored. So, as a PhD researcher, I posed a new question:

<b>How does engaging in familiar behaviors affect our ability to form new <abbr title="memories for specific past events">episodic memories</abbr>?</b>
<div align="center"><img src="/images/projects/motor_episode_interplay_uncropped.jpg" style="width:80%"></div>
<hr>
<h4>Research Design</h4>

To get at this question, I designed and built an original experimental paradigm that **180+** participants completed online. Each participant played a virtual memory game where they had to "run errands" through two different stores.

These errands involved two parts:

1. visiting a sequence of aisles (*by pressing buttons on the keyboard*)
2. collecting a sequence of objects (*by viewing each object for a few seconds*)

<div style="display: flex; align-items: center; justify-content: center;">
  <img src="/images/projects/scaffold_encoding.jpg" style="flex: 1; max-width: 90%; margin-right:0px;">
  <p style="flex:0.2;margin:0;text-align:center">
    <a href="https://app.gorilla.sc/task/12395634" target="_blank"><img src="/images/play_button.jpg" style="width:70%"></a>
    <br>
    ↑
    <br>
    <i>click to try!</i>
  </p>
</div>

Participants visited **two different stores** during the experiment. In one of these stores (**"predictable"**), the sequence of aisles was always the exact same — and participants memorized this sequence before the errands began. In the other (**"variable"**), the aisle sequence changed on each errand.

In this way, we created two learning contexts (or conditions): one where motor behavior during learning was consistent and familiar, and another where it was not. Aside from this distinction, the two contexts were identical. And note that our experiment made use of a <b>within-subjects design</b>, where all participants ran errands in both of the stores.

<div align="center"><img src="/images/projects/scaffold_conditions.jpg" style="max-width: 50%;"></div>

After participants ran their errands, we <b>tested their memory for the items they collected</b> in each store, looking at three different kinds of memory:
- memory for the **order** that items were collected during an errand 
- memory for the **aisle** that each item was collected from
- memory for the **individual items** themselves (i.e., their unique perceptual details)

For each test, the primary output variable was <b>memory accuracy</b>, or the *proportion of correct responses* each subject made throughout the experiment. We then looked at the difference in memory accuracy for items collected in the predictable vs. variable store, allowing us to see the effects of familiar behavior on memory.

<div align="center"><div class="emphasize">Our hypothesis was that memory accuracy would be better for items from the predictable store — because in this store, participants could "anchor" the novel items to the familiar sequence of actions.</div></div>

<hr>
<h4>Results</h4>

Across our three experiments, we found that **executing predictable behaviors during learning did indeed promote episodic memory**. And interestingly, this benefit was specific to memory for the **order** of items within an errand — not for their visual features or aisle they had been collected in.

The selectivity of this memory enhancement suggests that participants aren't simply paying better attention to *everything* that happens in the predictable store. Instead, following a familiar sequence of actions seems to specifically boost memory for the sequential structure of concurrent experience.

To explore the results yourself, check out the interactive <a href="https://shiny.posit.co/" target="_blank">Shiny</a> dashboard below.

<iframe height="600px" width="100%" frameborder="no" src="https://camillegasser.shinyapps.io/scaffold_rshiny/"
    style="border: 1px solid #e0e0e0; margin-top:5px; margin-bottom:5px"> </iframe>

<hr>
<h4>Key Outcomes</h4>

This project sits at the intersection of research on episodic memory and motor learning — two topics in cognitive psychology that have historically been studied in isolation. The novel finding that familiar behavior can enhance concurrent encoding suggests that there might be more cooperation between these learning systems than was once assumed. In the future, it might be possible to take advantage of this cooperation, designing therapeutic interventions for people who have dysfunction in one memory system, but not others — like what occurs in Alzheimer's.

For now, these findings simply highlight the important point that the actions we take throughout the day interact with the memories we create along the way.

> This research was:
> - published as a peer-reviewed manuscript in Psychological Science
> - presented at multiple international scientific conferences (e.g., Society for Neuroscience Annual Meeting)
> - **used as pilot data for a successful $90K+ grant from the National Institutes of Health (NIH)**

<hr>
<div align="center">
<h4>Research Process Overview</h4>
<img src="/images/projects/scaffold_pipeline.jpg" style="max-width: 99%; margin-top:5px">
</div>


<projtitle>Exposure to novelty in everyday life boots long-term memory</projtitle>

<h4>Motivation</h4>

During the COVID-19 pandemic, lives were uprooted. Lockdown protocols and stay-at-home orders narrowed the scope of our worlds, sharply reducing the excitement and variety we experienced on a day-to-day basis. People began lamenting that their memory felt blunted or distorted, with days blurring together and very few moments standing out as vivid or memorable.

What might've caused this memory dysfunction?

Controlled experiments conducted in the lab suggest that, at least in some cases, we remember more *novel* events better than more typical ones. Memory also tends to benefit from more *structure & variety* in one's experience, i.e., when there clear transitions between the different contexts we move through. (See <a href="https://davachilab.psychology.columbia.edu/sites/default/files/content/docs/TranscendingTime_2019_reduce.pdf" target="_blank">this paper</a> for more on this point!) For many of us, the pandemic reduced our exposure to both novelty and variety. 

In this study, we wanted to test this link between novelty and memory outside of the lab and in the real world, asking the critical question: **does having more novel experiences in everyday life lead to better autobiographical memory?**

<hr>
<h4>Research Design</h4>

For this study, we designed an **intensive longitudinal "daily diary" study** and recruited 50+ young adult participants to complete it. For two weeks, at the end of each day, each participant completed a daily survey (i.e., "daily diary"); they then completed a follow-up memory test another two weeks after the survey period.

All phases of the experiment were conducted remotely on <a href="https://www.qualtrics.com/" target="_blank">**Qualtrics**</a>, so participants could do each task conveniently on their phone, tablet, or computer.

<div align="center"><img src="/images/projects/diary_timeline.jpg" style="width:60%;margin-bottom:0px">
<figcaption>Schematic of study timeline.</figcaption>
</div>

During each daily diary, we asked a diverse set of questions about **what the participant had experienced that day**, including several metrics that tapped into how much **novelty** they were exposed to, such as:
<ul>
<li>whether they'd visited any new locations that day
<li>whether they'd interacted with any new people that day
<li>how typical or eventful they perceived their day to be
</ul>

We also asked people to describe **three specific events** that had occurred on that day. For each event, the participant provided an open-ended text description of what happened during the event (e.g., what they saw, felt, did) and assigned the event a unique title. They also described the **novelty of each event itself**, labeling it as either:
<ul>
<li>"routine" (something they experienced almost every day);
<li>"periodic" (something they experienced occasionally); or
<li>"new" (something never experienced before)
</ul>

<br>
<div align="center"><img src="/images/projects/diary_survey_design.jpg" style="width:80%;margin-bottom:0px">
<img src="/images/projects/diary_survey_qualtrics.jpg" style="width:40%;margin-bottom:0px">
<figcaption><b>TOP:</b> Overview of the content of questions asked during the daily diary survey, plus an example event title ("Baby without restraint") and written description.<br><b>BOTTOM:</b> Example view of diary questions on Qualtrics.</figcaption>
</div>

Two weeks after the daily diary period, we **tested participants' memory for the specific events they had described**, collecting both subjective and objective metrics of memory success.

<br>
<div align="center">
<img src="/images/projects/diary_memory_design.jpg" style="width:35%;margin-bottom:0px">
<img src="/images/projects/diary_qualtrics.jpg" style="width:20%;margin-bottom:0px;margin-left:20px">
<figcaption style="margin-bottom:0px"><b>LEFT</b>: Overview of questions asked during the memory test.<br><b>RIGHT</b>: Example view on Qualtrics.</figcaption>
</div>
<div class="emphasize">With this survey design, we can look at how the <b>novelty</b> embedded in a person's everyday life is linked to the <b>strength of their memory</b> for personal experiences.</div>

<hr>
<h4>Analysis & Results</h4>

Before diving into the relationship between novelty and memory, we wanted to get a general sense of the amount of new experiences participants engaged in throughout the study.

As is clear from the plots below, although novel experiences were somewhat less frequent than more typical ones, they were hardly uncommon amongst our participants.

<div align="center"><img src="/images/projects/diary_novelty_vars.jpg" style="width:90%;margin-bottom:0px">
<figcaption><b>Descriptive plots of all novelty-related variables.</b><br>
("Day-level" variables describe the participant’s behavior across the entire day.)<br>
("Event-level" variables describe the novelty of an individual event.)</figcaption>
</div>

Next, we took a look at performance on the memory test, where we asked participants to try and recall the events they'd reported during each daily diary.

As briefly mentioned above, we had two measures of memory performance:

<ol>
<li><b>memory vividness</b>: a 1-5 rating reflecting how vivid the participant perceive their memory to be
<li><b>event recall</b>: an open-ended text description of what the participant remembers about each event
</ol>

The first of these measures is already numeric, and therefore was pretty straightforward to analyze. But in order to work with event recall, we needed to transform the recall text into a quantitative metric reflecting memory quality.

To do this, we took inspiration from <a href="https://pubmed.ncbi.nlm.nih.gov/12507363/" target="_blank">existing audiobiographical memory scoring approaches</a> and modern advancements in large language models (LLMs) and **engineered a novel GPT-4 prompt** that was able to segment each text description into individual "chunks" — each of which represents a discrete piece of information contained in the recall (e.g., a person who was present, an action that was taken). Applying this prompt to each recall description allowed us to quantify the **number of unique details** participants remembered about each event.

<div align="center">
<img src="/images/projects/diary_recall_scoring.jpg" style="width:80%;margin-bottom:0px;margin-top:15px">
<figcaption>Illustration of recall scoring approach.</figcaption></div>

With recall scoring done, we could take a look at memory performance in our sample. Memory vividness ratings spanned the full 1-5 spectrum in a relatively uniform distribution, and most people recalled about 5-10 per event. 

These two metrics — vividness ratings and number of details recalled — were also strongly correlated with each other, suggesting that participants' subjective sense of the strength of their memories tracked closely with how much they actually remembered.

<div align="center">
<img src="/images/projects/diary_memory_vars.jpg" style="width:90%;margin-bottom:0px;margin-top:10px">
<figcaption><b>Descriptive plots of memory performance.</b><br>
Inset panel on the right shows the relationship between the two memory metrics: subjective recall vividness & the number of details recalled. *** p < .001</figcaption>
</div>

Now comes the critical question: was memory impacted by the novelty present in participants' lives?

Our analyses highlighted two key results:

<ol>
<li><b>More novel experiences were remembered more vividly and in greater detail than more typical events.</b>
</ol>

<p style="margin-left:40px">Events that were described as "new" during the diary period were remembered better than those that occurred "periodically", which in turn were remembered better than those that were "routine."</p>

<div align="center">
<img src="/images/projects/diary_event_novelty.jpg" style="width:60%;margin-bottom:5px;margin-top:15px">
<figcaption>Memory vividness and details recalled by event regularity.</figcaption></div>

<ol start="2">
<li><b>Doing something new boosted memory vividness for <i>other</i> events that happened on that day, even if those other events weren't novel themselves.</b>
</ol>

<p style="margin-left:40px">We found evidence for this claim through two analysis approaches. First, we ran a <b>multilevel linear regression model</b> where memory performance (vividness or details recalled) was predicted by <b>all</b> of the novelty-related variables we collected during the diary period. This analysis showed that some of these novelty-related variables — like visiting a new location or interacting with a new person on the same day an event occurred — led to better memory, even when controlling for the novelty of the event itself. </p>

<p style="margin-left:40px;margin-bottom:5px">Next, we filtered the data to look only at <b>routine and periodic events</b> and contrasted memory performance based on whether those events occurred <b>on the same day as a novel event</b>. This analysis showed that non-novel events that happened on the same day as a new event were remembered more vividly than those that did not.</p>

<div align="center">
<img src="/images/projects/diary_novelty_penumbra.jpg" style="width:85%;margin-bottom:0px;margin-top:15px">
<figcaption>Memory vividness and details recalled for <i>non-novel events</i> based on whether they occurred on the same day as a novel event or not.</figcaption></div>

<hr>
<h4>Conclusions & Next Steps</h4>

<b>Key Findings:</b>
<ul>
<li>Novel autobiographical experiences were remembered more vividly and in greater detail than those perceived as more typical. <li>The mnemonic benefit of engaging in a new experience extended across time, such that other events reported on the same day as a novel event — and/or on the same day as visiting a new location or interacting with somebody new — were also better remembered.
</ul>

These findings show the powerful and widespread effect that novelty has on our memory function, while also validating effects that have been observed in more controlled laboratory paradigms.

<b>Next Steps:</b>
<ul>
<li>Manuscript documenting this project is currently in progress.
<ul><li>In the meantime, check out Chapter 3 of <a href="/docs/camillegasser_dissertation_final.pdf" target="_blank">my dissertation</a> to read a full write-up of the study background, procedures, analyses, and results.
</ul>
<li>Ongoing analyses are exploring how <b>variety</b> in day-to-day activities — rather than novelty itself — impacts memory retention.
<li>The research team also plans to replicate these effects in a larger sample, while also expanding the age range of recruited participants to examine if the effects of novelty change across age.
</ul>

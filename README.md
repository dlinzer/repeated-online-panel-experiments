# How to use this repository.

This guide keeps a running list of the vendors, notes about them, and other practices for running repeated online panel experiments. [Testing Theories of Attitude Change With Online Panel Field Experiments](http://papers.ssrn.com/sol3/papers.cfm?abstract_id=2742869) describes the design and an application study. This repository assumes knowledge of that paper.

# Have a question about using the design?

Please use the [GitHub issues](https://github.com/dbroockman/repeated-online-panel-experiments/issues) feature to ask a question or make a request. Then you'll get a ping when we answer the question or update the repository to answer it.

# Vendors

## All-In-One Vendors

[Civiqs](https://www.civiqs.com/research) administers a one-stop shop.

## Vendors for Particular Pieces

### Survey Platform

For academic work, we prefer [Qualtrics](https://www.qualtrics.com). Most universities have a subscription.

[SurveyGizmo](https://www.surveygizmo.com) is a far cheaper alternative that may be preferable for practitioners. It is more difficult to program in our experience but workable.

### Tango Card - Incentives

Tango Card is a service that sends gift cards to respondents via email. A POST request is issued to the Tango API and the respodent gets the gift card. There are no fees. However, one's Tango account must be loaded with funds in advance. We recommend university researchers begin this process far in advance, as onboarding Tango with one's university and then getting the university to transfer funds can both take weeks.

The Tango API accepts a POST request, but Qualtrics and SurveyGizmo are only capable of placing GET requests. A simple go-between is available [here](tango). We set up a server that runs this go-between microservice.

Qualtrics does have a service called [Blast Rewards](https://www.qualtrics.com/innovation-exchange/tango-card/) that sends out gift cards to respondents using Tango. We are not familiar with how this works, but it may be easier than using the go-between.

### Brite Verify - Email Verification

We ask respondents for their email addresses and ask them to complete follow up surveys with solicitations sent to these email addresses. In our experience respondents often mistype their email addresses in the survey. We need them to correct these typos so that we can get their correct email. To detect these typos, we use the [BriteVerify](http://briteverify.com) API, which assesses whether an email address is valid. It can be integrated directly into Qualtrics. An example is available [here](briteverify).

### Twilio - Text Messages

We have sometimes given respondents the option of being texted when a follow-up survey is available. If they give their cell number and asked to be texted, we use a script available [here](twilio/invite_to_post_with_sms.py) to send text message with [Twilio](https://www.twilio.com).

### Google and Bing - Online Ads

Many respondents Google the name of the survey or Google the URL but do not enter the URL into their browser bars. Buying ads on Google and Bing allows one to redirect respondents to the survey. In our experience over 10% of respondents enter the survey via this mechanism. An example of the Google ad setup is available [here](baseline recruitment/search ads). Bing allows you to import Google ads, so we create the ads in Google first and then import them.

### Mail firm

We use [Snap Pack Mail](http://snappackmail.com).

# Recruitment Procedure

## Recruitment Letters

An example recruitment letter is available [here](baseline recruitment/Recruitment Letter Example 0 Baseline Multi Person HH.png). Note that recruitment letters are often addressed to multiple people in a household, each of whom gets a unique login code.

There are also [specs](baseline recruitment/specs.txt) to give mail firms for cheaper rates. We recommend using standard mail with a pre-cancelled stamp affixed, and non-profit mail if applicable. If the experiment is taking place in a concentrated area, use [PMOD](http://npf.org/blog/?p=1611) shipping. Note that standard and non-profit mail can take 2 weeks to arrive. We recommend leaving multiple weeks for mail to arrive. PMOD shipping can speed this up somewhat. First class mail is significantly more expensive but also much faster.

In addition, [this image](baseline recruitment/how long to keep survey open.png) provides an example of how survey response rates grow over time once mail arrives. Many people respond right away but over 40% of responses come in over a week after mail arrives. This suggests one should leave the survey open for at least two weeks after mail arrives.

The recruitment letters contain unique login codes for each individual and a website that redirects them to the survey.

## Website / Landing Page

The recruitment letter can direct respondents directly to the survey or to a landing page. [Here's](http://stanford.edu/~dbroock/stanford-berkeley-opinion-survey/Stanford-Berkeley_Survey_of_Public_Opinion/Stanford-Berkeley_Opinion_Survey.html) an example of a live landing page.

To register a domain (e.g., www.miamiopinion.org), you can use Amazon's [Route 53](https://en.wikipedia.org/wiki/Amazon_Route_53).

## Survey Questions

Survey questions usually fall into one of four buckets.

- Outcome measures
- Items might predict opinion change useful for modeling
- Filler questions unrelated to the broad outcome of interest (e.g., non-political questions)
- Ancillary non-experimental outcomes of independent interest

Example survey instruments are available [here](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/WKR39N) for a previous experiment using this design (see "Miami t0.pdf", etc.).

## Re-Survey Solicitation Emails

We ask respondents for their email addresses and ask them to complete follow up surveys with solicitations sent to these email addresses. Examples of these solicitaitons are available [here](reinterview recruitment).

# Treatment and Placebo Procedure

Our experimental design relies heavily on the use of placebo canvasses as a baseline comparison to a treatment canvass [(Nickerson 2005)](http://www.jakebowers.org/ITVExperiments/Nickerson.PA2005.pdf). When using the placebo procedure, it is necessary that the types of voters canvassed with the treatment script are similar to those canvassed with the placebo script. To do this, both treatment and placebo scripts should begin exactly the same. Only after a targeted voter is identified and marked as having come to the door should the scripts diverge to either treatment or placebo. Accurately marking the voter as having come to the door is essential for the experiment to work properly. We provide a sample placebo script and instructions to canvassers [here](placebo treatment).

# Other Resources

## Academic studies

- [Testing Theories of Attitude Change With Online Panel Field Experiments](http://papers.ssrn.com/sol3/papers.cfm?abstract_id=2742869) describes the design and an application study. This repository assumes knowledge of that paper.
- [Durably reducing transphobia](http://stanford.edu/~dbroock/published%20paper%20PDFs/broockman_kalla_transphobia_canvassing_experiment.pdf) was the first published experiment using the design.

## Power Calculator

The power calculator available [here](http://experiments.berkeley.edu) allows one to plan out a study using the repeated online panel design. We plan to open source the code for that webpage in this repository soon.

# FAQs

Q: If the design is used for a canvass experiment canvassers may need to walk for a small distance between households who answered the survey in order to deliver the treatment. How can this be dealt with?

A: Most subjects targeted with the baseline survey do not participate in it and will not be eligible for treatment.  In essence, the baseline survey has served as an oracle that reveals the many houses where, if the canvasser were to visit, their time would be wasted from the point of view of the experiment. We have found that when explaining the reason for low canvass turf density this way, canvassers readily understand that wasted time walking is better than wasted time both walking and talking.

---
layout: post
category: fun
title: "Instrumental Variables"
---


Ok. This is a hard area to see, and I wish there were classes that actually taught this stuff properly. So Short tour of quantitative social science: how do you measure if X affects Y?

This was a question I grappled with during senior year a lot. You will see why this is hard and this is the main reason I didn't go into social science grad school. Adding ola, lakshmi and amiya because this might be interesting to them too. Apologies for brevity and typos. I'm on iPad at conference. 

Basically in economics and sociology direct experimentation is hard. If you want to prove that x causes y, for example, or that 10% increase in x cause 20% increase in y, the ideal thing you Want to do is run a randomized controlled trial. (RCT) 

I.e. form two groups of people, at random (the randomness is important to make sure that another difference between the groups is unlikely to happen.) For example if group one is in Switzerland and group two is in france this is bad because maybe frenchness causes a 20% increase in y. So you find a group, divide them into two at random (so whether you're French or Swiss doesn't affect which group you get assigned to), then give one group 10% more x. Now you know the only difference is that you caused more x, so you can measure y. So you can say that x causes y for whatever result you find. This is how medical studies are done. There are things you want to control for (blinding, I.e. the patients don't know which group they are in, if it is a pill, it is just labeled pill a, and pill b. the doctors Also don't know, and finally when the experiment is finished, you collect the data and then Unblind the results, and look at it)

This is great in theory (and in medicine) but sometimes you want to do experiments on things that you can't control. So if you want to measure if abortions decrease crime, you can't just put together two groups and give one group more abortions (or maybe I'll find some black people and give one group syphillis and see what happens, I.e. Tuskegee experiments.) So what you look for is called "natural experiments". Say something extraordinary that caused this to happen. For example, if say two adjacent states legalized abortion at different times you can compare the towns right on the border.

But you now have to be very careful. Because there might be other reasons that the towns on the opposite sides of the border are different. For example, maybe they self select to be on one of the sides. So a good example is the two sides of Kansas City (maybe. I don't really know). A bad example is Greenwich vs Rye because Greenwich is deliberately on the other side for specific reasons (taxes). 

The really bad part is that if there is an underlying variance, then your study is completely useless. This is called omitted variable bias, or OVB. And is the bane of all quantitative social studies. The real problem with OVB is that you never know for sure If you missed something. Maybe there was some underlying cause that correlates with one of the groups and you missed it. 

Let me give you an example straight out of the freakonomics book. Say you want to see if charter schools help students. You compare charter school performance to regular public school. You see charter students do a lot better. So you conclude that it works. Done? But wait! There's more!
Now in Chicago, when they started charter schools they had students enter a lottery. If you won you got to go to the school, if you lost you went back to the regular school. So you look at the students who enrolled in the charter schools, this looks like a perfect experiment. The assignment is random. But some researchers found that charter school students performed the same as the public school students WHO LOST THE LOTTERY. I.e. simply entering the lottery improved your performance. Why? Because it means you have parents who care. And that improves performance a ton. 

So the original experiment was missing a variable: whether you were entered in the first place by parents who care. The new experiment found that. 

However, maybe there are MORE omitted variables... Who knows? There's no satisfying answer to this question except textual persuasion. And now were off the deep end.

Now flaws with the study, with Explanation inlined below. I've copied the article text.

-----
Theodore Vasiloudis writes:

I came upon this article by Laura Hamilton, an assistant professor in the University of California at Merced, that claims that “The more money that parents provide for higher education, the lower the grades their children earn.”

I can’t help but feel that there something wrong with the basis of the study or a confounding factor causing this apparent correlation, and since you often comment on studies on your blog I thought you might find this study interesting.

My reply: I have to admit that the description above made me suspicious of the study before I even looked at it. On first thought, I’d expect the effect of parent’s financial contributions to be positive (as they free the student from the need to get a job during college), but not negative. Hamilton argues that “parental investments create a disincentive for student achievement,” which may be—but I’m generally suspicious of arguments in which the rebound is bigger than the main effect.



Basically you first expect that richer students would do better than poor students for obvious reasons. Tis study finds the reverse. This is surprising! Lets look at the data. 
So I clicked through to the study and indeed found a problem, and it’s the one you might expect if you follow this sort of thing. Hamilton regresses college grades on the amount parents spent on college, and finds a negative correlation: more parental spending is associated with lower grades.

The problem I’m worrying about is selection, what is sometimes called “survivorship bias.” If you’re not doing so well in college but it’s being paid for, you might stay. But if you’re paying for it yourself, you might just quit. This would induce the negative correlation in the data, but not at all through the causal story that Hamilton is telling, that students who are parentally-supported “dial down their academic efforts.” Couldn’t it just be that these students are working as hard as they ever would, they’re just more likely to stay in school?

Basically if you suck and you're poor, you drop out cause it costs too much. But if you're rich, daddy might pay for you to spend 6 years at college. And this is what causes the negative effect of money in grades to look this good. So 

1. the study results which says "spending more reduces grades" is backwards. The causality direction is reversed. Really it is "lower grades requires higher spending". 
2. Spending more might reduce grades CONDITIONAL on being in school. 

Another thing that frustrated me today because they had a panel on "why you should do high risk research for your phd" here at this conference. A panel of really distinguished professors stood up and said "look I did really high risk things and it paid off look I'm now a professor at ..."

But the truth is, doing high risk things is great conditional to being a professor. I.e. where were the people who did high risk things and failed? You have to look at everyone who did high risk things. Not just the high risk things where they succeed, become professors and then sit on panels. 


To say it another way: it seems completely plausible to me that (a) there could be a negative correlation between parental support and student grades, conditional on students being in college, but (b) paying more of your college student’s tuition would not (on average) lower his or her grades.

Translation, the people dropping out are affecting the metrics. 

Yes, Hamilton runs an analysis controlling for students’ college admissions test scores, but (a) admissions test scores don’t tell the whole story about students’ abilities and readiness for college, and (b) she also controls for post-treatment variables such as student’s major, full-time status, and—amazingly—whether the student is employed during school. You shouldn’t control for these things! You really really shouldn’t control for whether a student is working during school, if you’re trying to estimate the effect of parental support.

Basically you shouldn't control for things that could separate your groups. Naively someone might think "controlling for more things is better!"  But this is not true. For example, maybe parents who spend more also buy their kids a BMW. Now you shouldn't control for whether people had a BmW at college if you are trying to measure whether spending more is bad for you. The BMW is part of the thing you're measuring, not a separate thing to control for.

Hamilton also performs an analysis on a different dataset including within-student comparisons when student aid varied over time. This is fine but it has the same problem that it doesn’t seem that students get counted after they drop out. Again, an observed correlation does not necessarily correspond to a causal effect—and I don’t say this in a vague “correlation does not equal causation” sense but more specifically that there’s a clear selection problem going on here (as well as a comparison conditional on intermediate outcomes). Hamilton writes, “These results provide strong evidence that selectivity processes are not driving the negative relationship between parental aid and GPA,” but I don’t see it.

As a side note, I’m slightly bothered by this graph from the paper:

Screen Shot 2013-01-22 at 7.13.23 PM

I mean, sure, a graph is better than a table full of numbers such as “-11.503***,” but . . . are there really many families making $5,000 or $15,000 a year and giving their kids $40,000 to pay for college? I guess it’s possible but it seems unlikely to me. A footnote says, “The funds needed to provide parental aid can come from a variety of sources . . . Parents may thus offer funds that exceed income earned in a given year.” Still, I can’t imagine it happens that often. And, when it does, I’d expect the impact on the student to be different from funds supplied by a richer family. I can’t imagine it feels so much like free money to a student if parents are paying more for college than they (the parents) make in a year. So, to the extent the graph makes sense and even if I were to believe the author’s data analysis and theoretical model, I wouldn’t believe the model would hold in this case.

Blah blah ignore this part above. 
I’m surprised the reviewers for the American Sociological Review didn’t catch these problems, which are pretty standard issues in causal inference: survivorship bias and controlling for intermediate variables. Or maybe they wanted to publish the paper because it represents empirical work—even if flawed—on an important topic.

Translation: because OVBs are such a big issue, this one single flaw could invalidate the entire study. 
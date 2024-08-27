---
layout: distill
title: "into the lion’s den: statline scouting the seibu lions farm’s pitchers"
description: in the jungle, the mighty jungle, the lion sleeps tonight. but which lions are YOU sleeping on?
categories: personal baseball npb
date: 2024-06-29

authors:
  - name: Patrick Ramos
    url: "https://ramos-ramos.github.io/"
    # affiliations:
    #   name:

  - name: Ryan Ramos
    url: "https://ramos-ramos.github.io/"
    # affiliations:
    #   name:

toc:
  - name: "Into the lions den"
  - name: "1. Shinya Sugai"
  - name: "2. Haruki Sugiyama"
  - name: "3. Towa Uema"
  - name: Rotation of the future?

thumbnail: /assets/img/seibu_statline_scouting/seibu_statline_scouting.png
---

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/seibu_statline_scouting/seibu_statline_scouting.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


## Into the lion’s den

Tatsuya Imai. Kaima Taira. Kona Takahashi. Chihiro Sumida. The Saitama Seibu Lion’s pitching lab has produced a stacked rotation. This is all without mentioning PL ROY frontrunner Natsuki Takeuchi and their [NPB #4 prospect “the Randy Johnson of Japan” Shinnosuke Hada](https://twitter.com/yakyucosmo/status/1742538964089901430). With this incoming youth movement, the question is: just how deep does the lion’s den go?

We take a crack at [Foolish Bailey-style Statline Scouting](https://www.youtube.com/watch?v=KHR6ODadQJ8&list=PL3lJFzNSaN7RHuqCVSue8ewUx8lqyZkjU) the Lions farm looking for pitching prospects. In the spirit of statline scouting, we’ll be excluding the most intuitive choices, Natsuki Takeuchi and Shinnosuke Hada.

*All advanced stats are as of June 16, 2024. Other data like game logs may go past that.*

## 1. Shinya Sugai

| Player     |   Age | Pos.   |   IP |   FIP |   FIP- |   K% |   BB% |   K-BB% |
|:-----------|------:|:-------|-----:|------:|-------:|-----:|------:|--------:|
| Shinya Sugai   |    20 | SP     | 44   |  **2.57** |     70 | 24.9 |   7.2 |    **17.7** |
| Shinnosuke Hada |    20 | SP     | 31   |  2.91 |     79 | 26.3 |  12.7 |    13.6 |
| Haruki Sugiyama   |    18 | SP     | 17   |  3.01 |     82 | 17.5 |   1.6 |    15.9 |
| Kaito Yoza   |    28 | SP     | 51.3 |  3.42 |     93 | 12.4 |   5.7 |     6.7 |
| Towa Uema   |    23 | SP     | 32   |  3.52 |     96 | 12   |   6   |     6   |

The most obvious thing to do is simply to sort by a strong performance-indicator like FIP or something adjacent like K-BB%. Filtering to pitchers with at least 15 IP, it turns out that the top performers on the Lions farm are actually the same person. And perhaps surprisingly, it’s not their top prospect Shinnosuke Hada. The Lions’ farm leader in FIP and K-BB% is 20-year-old Shinya Sugai, a 2021 3rd round developmental (*ikusei*) draft pick. This year Sugai has skimmed his BB% from 8.7 to 7.2 and bumped his K% from 18.5 to 24.9. A 24.9 K% and 17.7 K-BB% might not be off-the-scale numbers, but his 2.57 FIP is good for a 70 FIP-. Note that his K% is second only to Hada, and Sugai walks less batters. 

<div class="l-body" align="center">
  <iframe src="{{ '/assets/plotly/seibu_statline_scouting/farm_k-bb.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>
<div class='caption'>Sugai’s K-BB% is good even compared to players outside of the Seibu Lions.</div>

Sugai is not only impressive within his own team, but also across the entire farm level of NPB. Among farm pitchers with at least 25 IP, Sugai is in the 96th percentile in K-BB%.

<div class="l-body" align="center">
  <iframe src="{{ '/assets/plotly/seibu_statline_scouting/sugai_np_ip.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>
<div class='caption'>Sugai could still work on his efficiency.</div>

One thing to bear in mind is that Sugai has issues with pitching deep into games, usually reaching 90+ pitches by the end of the 5th inning and making it to the 6th inning or further on only 2 out of 10 appearances on the farm. Furthermore, Sugai already has made an appearance on the top team, where he struggled against major league hitting, giving up 4 walks to his 4 strikeouts alongside a home run over 5 innings in a start.

## 2. Haruki Sugiyama

| Player   |   Age | Pos.   |   IP |   FIP |   xFIP |   K% |   BB% |   K-BB% |
|:---------|------:|:-------|-----:|------:|-------:|-----:|------:|--------:|
| Haruki Sugiyama |    18 | SP     | 17   |  3.01 |   **2.74** | 17.5 |   **1.6** |    15.9 |
| Kaito Yoza |    28 | SP     | 51.3 |  3.42 |   3.72 | 12.4 |   5.7 |     6.7 |
| Uema Towa |    23 | SP     | 32   |  3.52 |   3.56 | 12   |   6   |     6   |
| Shinya Sugai |    20 | SP     | 44   |  2.57 |   2.83 | 24.9 |   7.2 |    17.7 |
| Yuto Akagami |    25 | RP     | 18   |  4.48 |   4.99 |  6.8 |   8.1 |    -1.4 |

<div class="l-body" align="center">
  <iframe src="{{ '/assets/plotly/seibu_statline_scouting/sugiyama_k_bb.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>
<div class='caption'>Sugiyama may not strike out as many batters as Hada or Sugai, but he keeps the walks so low that he’s second in K-BB%.</div>

Another thing to try is to see who has the lowest walk-rate. Who’s avoiding free base-runners the most? With the same minimum 15 IP among Lions farm pitchers, it’s not Hada again, and neither is it Sugai. 2023 3rd rounder Haruki Sugiyama leads the farm with a 1.6 BB% at just 18 years old, which incidentally gives him the second highest K-BB% at 15.9. He also happens to lead in xFIP at 2.74. While he’s not striking out batters at a significant clip, across 3 games on the farm where he’s gone at least 5 innings deep each, he’s given up only 1 walk. What Sugiyama lacks in stuff, he makes up for in control

<div class="l-body" align="center">
  <iframe src="{{ '/assets/plotly/seibu_statline_scouting/farm_bb.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>
<div class='caption'>Sugiyama’s control stands out even compared to the rest of NPB’s farm.</div>

If we were to widen our scope to the entire NPB farm system, Sugiyama’s control is still evident, with him ranking in the 97th percentile in walk rate among pitchers with at least 15 IP.

## 3. Towa Uema

| Player     |   Age | Pos.   |   IP |   FIP |   xFIP |   FIP- |   xFIP- |   GB% |   OFFB% |
|:-----------|------:|:-------|-----:|------:|-------:|-------:|--------:|------:|--------:|
| Towa Uema   |    23 | SP     |   32 |  3.52 |   3.56 |     96 |      97 |  53.6 |    32.6 |
| Hiroki Inoue  |    22 | RP     |   15 |  3.77 |   4.12 |    102 |     112 |  50.9 |    28   |
| Minato Aoyama |    23 | SP     |   49 |  4.2  |   4.03 |    114 |     109 |  52.8 |    30.7 |
| Yutaro Watanabe |    23 | SP     |   24 |  4.28 |   4.63 |    116 |     126 |  54.9 |    28.4 |

Aside from strikeouts and walks, we can also take a look at batted ball distributions. If we filter to Lions farm pitchers with a minimum 15 IP who have a GB% over 50 and an OFFB% less than or equal to 33, only 2019 7th round pick Towa Uema has a FIP- and xFIP- below 100. While the 23-year-old isn’t putting up the gaudiest strikeout and walk numbers, his ability to keep batted balls more on the ground and less in the air has helped him get to a 2.12 ERA.

<div class="l-body" align="center">
  <iframe src="{{ '/assets/plotly/seibu_statline_scouting/uema_gb_offb_ip.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>
<div class='caption'>If you discount the lack of IP in 2023, Uema’s batted ball distribution is getting better and better.</div>

It’s hard to evaluate whether Uema truly fits the profile of a groundball pitcher since he missed 2022 with elbow surgery and only pitched 12 2/3 last year, but across his last three seasons, his GB% has consistently gone up (44.3 → 47.6 → 53.6) while his OFFB% has consistently gone down (41.1 → 37.3 → 32.6). He’s also currently putting up the best walk rate of his career at 6 BB%.

<div class="l-body" align="center">
  <iframe src="{{ '/assets/plotly/seibu_statline_scouting/farm_gb_offb.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>
<div class='caption'>Uema is one of a select group with ≤ 33 GB% and >50 OFFB%.</div>

Uema’s batted ball profile puts him among the 20% of all farm pitchers with at least 25 IP who have a GB% greater than 50 and an OFFB% less than or equal to 33. His GB% lies in the 90th percentile while his OFFB% is in the 81st percentile.


<!-- ## 4. Taishi Mameda

Lowering our innings threshold to only 10, we can find a reliever with serviceable FIP and xFIP. 2020 fourth round pick Taishi Mameda’s 3.75 ERA on the farm may seem underwhelming, but he’s underperforming a 2.70 FIP and a 2.98 xFIP. The 21-year-old’s 5.27 ERA at the major level seems even less impressive, but most of that can be attributed to a poor start to the season, which included a 4 ER clunker over just 1 IP. In his last 8 appearances on the top team, he’s only given up 1 ER over 9 IP, putting him more in line with his 2023 season statline of a 0.59 ERA over 15 1/3 IP.
 -->

## Rotation of the future?

It’s important to remember that these are all prospects. They may pan out, they may not. But one thing for sure is that they’re putting up interesting numbers down on the farm. It’s certainly fun to think that perhaps one day, these pitchers will contribute to keeping the Saitama Seibu Lions a fearsome, dangerous lion’s den.

### Data Sources:

 - [ぼおのの日記](https://bo-no05.hatenadiary.org/)
- [ベースボール週刊](https://sp.baseball.findfriends.jp/)




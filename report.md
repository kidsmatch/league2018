---
title: "队员报告"
---

本页面构建于 {{ site.time }}
{% assign players = site.data.records | group_by:"player" %}

# KidsLeague2018 队员分析报告
### [回到主页](index.html)


<table>
  <tr>
    <th>队员</th>
    <th>报告</th>
  </tr>
  {% for player_and_matchs in players %}
  {% assign player = site.data.players | where: "player", player_and_matchs.name | first %}
  {% assign matchs = player_and_matchs.items %}
  {% assign s_mvp = matchs | where: "mvp", "yes" | size %}
  {% assign s_win = matchs | where: "result", "win" | size %}
  {% assign s_lose = matchs | where: "result", "lose" | size %}
  {% assign s_mvp_win = matchs | where: "mvp", "yes" | where: "result", "win"  | size %}
  {% assign s_mvp_lose = matchs | where: "mvp", "yes" | where: "result", "lose"  | size %}
  {% assign s_score = 0 %}
  {% assign min_score = 100 %}
  {% assign min_attack = 100 %}
  {% assign min_pain = 100 %}
  {% assign max_score = 0 %}
  {% assign max_attack = 0 %}
  {% assign max_pain = 0 %}
  {% assign s_attack = 0 %}
  {% assign s_pain = 0 %}
  {% assign size = matchs | size %}
  {% assign k = 0 %}
  {% assign d = 0 %}
  {% assign a = 0 %}
  {% assign match_count = 0.0 %}
  {% assign team_k = 0 %}  
  {% assign player_real_score = 0 %}
  {% assign player_max_score = 0 %}
  {% for match in matchs %}  
    {% assign min_score = match.score | at_most : min_score %}
    {% assign max_score = match.score | at_least : max_score %}
    {% assign s_score = s_score | plus: match.score  %}
    {% assign s_attack = s_attack | plus: match.attack %}
    {% assign s_pain = s_pain | plus: match.pain %}
    {% assign k = k | plus: match.K %}
    {% assign d = d | plus: match.D %}
    {% assign a = a | plus: match.A %}
    {% assign team_k = team_k | plus: match.matchK %}
    {% assign match_count = match_count | plus: 1 %}
    {% assign score_per_match = match.matchId |minus:1| divided_by: 12 | plus: 1 %}
    {% if match.result == "win" %}
      {% assign player_real_score = player_real_score | plus: score_per_match %}
    {% endif %}
    {% assign player_max_score = player_max_score | plus: score_per_match %}
  {% endfor %}
  {% assign avg_score = s_score | divided_by: size| round:1 %}
  <tr>
    <td>  {{player.wx}}  <br>  ({{player.team}}) </td>  
    <td>  
{{player.player}}({{player.wx}}), 来自{{player.team}}
<br>
<br>共出场{{size}}次，赢了{{ s_win }}场，输了{{ s_lose }}场。
<br>为团队获得{{team_real_score}}分中的{{player_real_score}}分，贡献率{{gongXian}}%,
<br>如果自己上的场次全胜，自己可以拿{{player_max_score}}分，拿分效率{{xiaoLv}}%.
<br>
<br>共得到{{s_mvp}}次MVP, 其中{{s_mvp_win}}次胜方MVP,{{s_mvp_lose}}次败方MVP。
<br>共击杀{{k}}次，助攻{{a}}次，合计{{ka}}次。死亡{{d}}次。 KDA值为{{kda}}。参团率为{{canTuan}}%。
<br>
<br>平均评分{{avg_score}},其中最高评分{{max_score}},最低评分{{min_score}}。
<br>平均输出{{avg_attack}},最高值{{max_attack}},最低值{{min_attack}}。
<br>平均承伤{{avg_pain}},最高值{{max_pain}},最低值{{min_pain}}。 
    </td>
  </tr>
  {% endfor %}
</table>



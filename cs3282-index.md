<frontmatter>
title: CS3282 - {{ year }} Batch
pageNav: 2
</frontmatter>

# CS3282 - {{ year }} Batch

{% set projects = [
    {name: 'CATcher', students: [
        ['LOW JUN KAI, SEAN', 'seanlowjk', 'A1', 'B2', 'C2'],
        ['DING YUCHEN', 'dingyuchen', 'A1', 'B1', 'C1']
    ]},
    {name: 'MarkBind', students: [
        ['RYO CHANDRA PUTRA ARMANDA', 'ryoarmanda', 'A2', 'B1', 'C1'],
        ['JONAH TAN JUN ZI', 'jonahtanjz', 'A2', 'B2', 'C2']
    ]},
    {name: 'RepoSense', students: [
        ['HSU ZHONG JUN', 'dcshzj', 'A2', 'B1', 'C1'],
        ['CHAN GER HEAN', 'gerhean', 'A1', 'B1', 'C1']
    ]},
    {name: 'TEAMMATES', students: [
        ['TAN CHEE PENG', 't-cheepeng', 'A1', 'B2', 'C2'],
        ['MO ZONGRAN', 'moziliar', 'A2', 'B1', 'C1'],
        ['LIM ZI WEI', 'halfwhole', 'A2', 'B2', 'C2'],
        ['LI JIANHAN', 'jianhandev', 'A1', 'B2', 'C2']
    ]}
] %}

{% for project in projects %}
**{{ project.name }}:**
{% for student in project.students %}* [{{ student[0] }}](#{{ student[0] | lower | replace(' ', '-') | replace(',', '')}})
{% endfor %}
{% endfor %}

{% for project in projects %}
# {{ project.name }}
  {% for student in project.students %}

<include src="students/{{ student[1] }}/studentInfo.md" boilerplate >
  <span id="name">{{ student[0] }}</span>
  <span id="folder">{{ student[1] }}</span>
  <span id="mod">cs3282</span>
</include>
  {% endfor %}
{% endfor %}

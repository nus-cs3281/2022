<frontmatter>
title: CS3281&2 - {{ year }} Batch
pageNav: 2
</frontmatter>

# CS3281 - {{ year }} Batch

{% set projects = [
    {name: 'CATcher', students: [
        ['Goh Yee Chong, Gabriel', 'gycgabriel', 'A2', 'B3', 'C3'],
        ['Lee Chun Wei', 'chunweii', 'A4', 'B2', 'C2'],
        ['Lee Xiong Jie, Isaac', 'luminousleek', 'A1', 'B3', 'C3']
    ]},
    {name: 'MarkBind', students: [
        ['Hannah Chia Kai Xin', 'kaixin-hc', 'A4', 'B2', 'C2'],
        ['Jovyn Tan Li Shyan', 'jovyntls', 'A4', 'B1', 'C1'],
        ['LIU YONGLIANG', 'tlylt', 'A1', 'B1', 'C1'],
        ['ONG JUN XIONG', 'ong6', 'A3', 'B1', 'C1']
    ]},
    {name: 'RepoSense', students: [
        ['CHAN JUN DA', 'chan-j-d', 'A3', 'B4', 'C4'],
        ['Gokul Rajiv', 'gok99', 'A2', 'B1', 'C1'],
        ['TAY YI HSUEN', 'yhtMinceraft1010X', 'A3', 'B4', 'C4'],
        ['Zhou Jiahao', 'Zhou-Jiahao-1998', 'A1', 'B1', 'C1']
    ]},
    {name: 'TEAMMATES', students: [
        ['Chang Weng Yew, Nicolas', 'Nicolascwy', 'A3', 'B3', 'C3'],
        ['FANG JUNWEI, SAMUEL', 'samuelfangjw', 'A3', 'B2', 'C2'],
        ['JAY ALJELO SAEZ TING', 'jayasting98', 'A2', 'B4', 'C4'],
        ['LIU ZHUOHAO', 'fsgmhoward', 'A1', 'B2', 'C2'],
        ['MOK KHENG SHENG FERGUS', 'FergusMok', 'A1', 'B3', 'C3'],
        ['ZHAO JINGJING', 'zhaojj2209', 'A2', 'B2', 'C2'],
        ['Zhang Ziqing', 'ziqing26', 'A2', 'B3', 'C3']
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
  <span id="mod">cs3281</span>
</include>
  {% endfor %}
{% endfor %}

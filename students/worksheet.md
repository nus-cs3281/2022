<frontmatter>
footer:
</frontmatter>
{% from "index.md" import projects %}

# Feedback for Round A Slides

{% macro print_student(student) %}
<div class="container">
 <div class="row">
  <div class="col-sm-3">

<img src="{{ baseUrl }}/students/{{ student[1] }}/photo.png" height="150" /><br>
 </div>
  <div class="col">

##### **For:** {{ student[0] }} `@{{ student[1] }}`

##### **Reviewer:**
  </div>
 </div>
</div>


<box>

**1. Create visual slides**
* Use slides for visual support only
* Minimize text
* Eliminate visual clutter
</box>


- [ ] Content can be made more visual e.g. %%..................%%<br><br>
- [ ] Too much text in slides e.g. %%..................%%<br><br>
- [ ] Slide background can be improved e.g. %%..................%%<br><br>

<box>

**2. Make your points clear**
* Use assertion-evidence format
* Make key point stand out
* Slip in the definitions
* Show before you tell

</box>

- [ ] The _assertion-evidence_ format can help in some places e.g. %%..................%%<br><br>
- [ ] Slides containing key points can be made to stand out more e.g. %%..................%%<br><br>
- [ ] Some jargon can be eliminated e.g. %%..................%%<br><br>
- [ ] Order of the content can be improved 'show' more before 'tell' e.g. %%..................%%<br><br>

<box>

**3. Animate meaningfully**
* Use animation to direct attention
* Minimize ‘change of scene’

</box>

- [ ] Can use more/less/different animations 
  * %%..................%%<br><br>
  * %%..................%%<br><br>
  * %%..................%%<br><br>
- [ ] Can use better 'continuity' between related slides e.g. %%..................%%<br><br>

<box>

**4. Use the _click-look-recall_ technique of delivery**

</box>

- [ ] Add visuals to correspond to each point e.g. %%..................%%<br><br>

{% endmacro %}

{% macro print_feedback_table() %}

{% for project in projects %}
{% for student in project.students %}
{{ print_student(student) }}
<hr>
{{ page_break }}
{% endfor %}
{% endfor %}
{% endmacro %}

{{ print_feedback_table() }}


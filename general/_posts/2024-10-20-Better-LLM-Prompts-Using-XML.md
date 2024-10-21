---
layout: post
title: Better LLM Prompts Using XML
description: >
  This article explains why using XML solves some problems with prompting and provides better results from an LLM.
sitemap: true
---

Large Language Models (LLMs) have revolutionized natural language processing, but crafting effective prompts can be challenging. Unstructured prompts often lead to inconsistent results. Enter XML-structured prompts. XML-structured prompts enhance LLM interactions by improving clarity, accuracy, and parsability of AI responses. This concept is even more critical when using smaller models.

Until recently, I've been asking LLM's questions about writing code, or open ended questions to expand on ideas for chapter content. Large LLM's such as Claude and ChatGPT are great for this purpose. 

When I starting diving into using smaller locally-run LLM's for parsing pentest scan data, I found that it's critical to use carefully designed prompts with them. That's when I read about using XML to design LLM prompts. I've found XML to work best with prompting the smaller LLM's for specific tasks.

## The XML Advantage

XML (eXtensible Markup Language) offers several benefits when structuring LLM prompts:

1. **Clear Delineation**: XML tags provide distinct boundaries between different components of a prompt, reducing ambiguity.

2. **Hierarchical Organization**: Nested tags allow for logical grouping of related information.

3. **Improved Parsing**: XML-structured responses are easier to process programmatically.

4. **Consistency**: Standardized tags promote uniformity across different prompts and projects.

5. **Flexibility**: XML schemas can be easily modified or extended to accommodate new requirements.

Let's explore two practical examples of XML-structured prompts to illustrate these benefits.

## Example 1: Python Code Generation Prompt

Here's an example of how to structure a prompt for generating Python code:

```xml
<prompt>
  <task>Generate Python code</task>
  <description>Create a function that calculates the Fibonacci sequence up to a given number</description>
  <requirements>
    <item>Use recursion</item>
    <item>Include type hints</item>
    <item>Add docstring with examples</item>
  </requirements>
  <output_format>
    <language>python</language>
    <version>3.9+</version>
  </output_format>
</prompt>
```

This structured prompt clearly defines the task, specific requirements, and desired output format. The LLM can now generate a more accurate and tailored response based on these well-defined parameters.

## Example 2: Cybersecurity Analysis Prompt

For cybersecurity engineers, here's an example of a prompt structure for analyzing potential vulnerabilities:

```xml
<prompt>
  <task>Vulnerability analysis</task>
  <target_system>
    <name>E-commerce web application</name>
    <technology_stack>
      <frontend>React</frontend>
      <backend>Node.js</backend>
      <database>MongoDB</database>
    </technology_stack>
  </target_system>
  <focus_areas>
    <item>SQL injection</item>
    <item>Cross-site scripting (XSS)</item>
    <item>Authentication bypass</item>
  </focus_areas>
  <output_format>
    <structure>
      <section>Vulnerability description</section>
      <section>Potential impact</section>
      <section>Recommended mitigation steps</section>
    </structure>
    <priority_scale>
      <low>1-3</low>
      <medium>4-7</medium>
      <high>8-10</high>
    </priority_scale>
  </output_format>
</prompt>
```

This prompt provides a comprehensive structure for analyzing vulnerabilities in a specific system. It clearly defines the target, areas of focus, and desired output format, enabling the LLM to generate a more focused and actionable security analysis.

By adopting XML-structured prompts, developers and cybersecurity professionals can reach the full potential of LLMs, obtaining more precise, consistent, and easily parseable responses. This approach not only enhances the quality of AI-generated content but also streamlines the integration of LLM outputs into existing workflows and applications[1][4].

As the field of prompt engineering continues to evolve, XML-structured prompts will become a standard practice, offering a foundation for more sophisticated and reliable AI-driven solutions[2][3].

Citations:

[1] https://xmlaficionado.com/XML+Aficionado/2024/04/Using+XML+Schema+in+AI+System+Prompts

[2] https://www.reddit.com/r/LocalLLaMA/comments/1dsuzn8/finetuning_a_llm_on_xml_files/

[3] https://github.com/Bradybry/chatXML

[4] https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags
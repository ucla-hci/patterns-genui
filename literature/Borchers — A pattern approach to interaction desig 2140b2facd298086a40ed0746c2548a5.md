# Borchers — A pattern approach to interaction design

```
@inproceedings{10.1145/347642.347795,
author = {Borchers, Jan O.},
title = {A pattern approach to interaction design},
year = {2000},
isbn = {1581132190},
publisher = {Association for Computing Machinery},
address = {New York, NY, USA},
url = {https://doi.org/10.1145/347642.347795},
doi = {10.1145/347642.347795},
abstract = {To create successful interactive systems, user interface designers need to cooperate with developers and application domain experts in an interdisciplinary team. These groups, however, usually miss a common terminology to exchange ideas, opinions, and values.This paper presents an approach that uses pattern languages to capture this knowledge in software development, HCI, and the application domain. A formal, domain-independent definition of design patterns allows for computer support without sacrificing readability, and pattern use is integrated into the usability engineering life cycle.As an example, experience from building an award-winning interactive music exhibit was turned into a pattern language, which was then used to inform follow-up projects and support HCI education.},
booktitle = {Proceedings of the 3rd Conference on Designing Interactive Systems: Processes, Practices, Methods, and Techniques},
pages = {369–378},
numpages = {10},
keywords = {pattern languages, music, interdisciplinary design, exhibits, education, design methodologies},
location = {New York City, New York, USA},
series = {DIS '00}
}
```

# One Sentence

---

# More Sentences

---

# Key Points

---

### Design patterns follow a hierarchical structure

> … high-level patterns describing the context in which it can be applied, and lower-level patterns that could be used after the current one to further refine the solutions
> 

### What a pattern should be like based on an example in architecture

> A meaningful, concise *name* identifies the pattern, a *ranking* indicates the validity of the pattern, a *picture* gives a “sensitizing" and easily understood example of the pattern applied, and the *context* explains which larger patterns it helps to implement. Next, a short *problem statement* summarizes the competing “forces", or design tradeoffs, and a more extensive *problem description* gives empirical background information, and shows existing solutions.

The following *solution* is the central pattern component. It generalizes the examples into a clear, but generic set of instructions that can be applied in varying situations. A *diagram* describes this solution and its constituents graphically, and *references* point the reader to smaller patterns that can be used to implement this pattern.
> 

To sum up—

> Name, context, problem, solution, examples, diagram, and cross-references are still the essential constituents of each pattern.
> 

### Context and references

A pattern P1 might consists of employing other patterns, say P2:

- P1 is P2’s context (P1 indicates when/where P2 would be used)
- P2 is P1’s reference (P1 need to refer to contents of other patterns)

> Each pattern has a **context** represented by edge pointing to it from higher-level patterns. They stretch the design situations in which it can be used. Similarly, its **references** show what lower-level patterns can be applied after it has been used. This relationship creates a *hierarchy*  within the pattern language. It leads the designer from patterns addressing large-scale design issues, to patterns about small design details, and helps him locate related patterns quickly.
> 

# Other Notes

---

### A psychological effect of framing certain designs a pattern

> This idea of creating a vocabulary implements well-known results from psychological research about *verbal recoding*: “When there is a story or an argument or an idea that we want to remember [ … ], we make a verbal description of the event and then remember our verbalization”
> 

# Take-Away

---

### Using LM to extract and synthesize design patterns

Given a set of papers, we can use LM (e.g., Notebook LLM) to extract and synthesize design patterns
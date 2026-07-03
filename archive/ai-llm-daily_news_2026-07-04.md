# AI / LLM Daily — 2026-07-04

## Show HN: Mcpsnoop – Wireshark for MCP (transparent proxy and live TUI)

**Source:** Hacker News  
**URL:** https://github.com/kerlenton/mcpsnoop  
**Topics:** Agents, AI Coding, Hands-on, Claude / Anthropic

A new open-source tool called mcpsnoop, written in Go, positions itself as a Wireshark-equivalent for the Model Context Protocol. It works as a transparent proxy that sits between an AI client and MCP servers, displaying every tool call in a live terminal UI as it happens. The tool addresses a specific blind spot in MCP development. The official MCP Inspector connects as its own client, meaning it never observes the actual traffic flowing between a real client like Claude Desktop, Cursor, or Claude Code and the MCP server being developed. A breakpoint in the server code only fires once a request arrives, which cannot reveal calls the client never made or calls made with unexpected arguments. This matters in practice because MCP debugging often degenerates into tailing log files in a tmp directory and guessing when tools are silently not called, when capability negotiations do not line up, or when a call hangs without explanation. By sitting transparently in the middle, mcpsnoop captures and displays the full bidirectional communication without requiring any code changes to either client or server. The project appeared as a Show HN post and has attracted early community attention with 44 GitHub stars. It ships as a single binary with a terminal-based user interface that renders intercepted protocol messages in real time. For developers building MCP servers, an increasingly common task as Claude Code, Cursor, and other agent-based tools adopt the protocol, this addresses one of the most frustrating aspects of the development loop: understanding exactly what the client is requesting and why a server might not be responding as expected.

**Why it matters:** MCP server development currently lacks visibility into what clients actually send over the wire, forcing developers to debug by inference. Anyone building or maintaining MCP servers for Claude Code or Cursor now has a drop-in proxy that reveals exact protocol exchanges without modifying client or server code.

---

## Using DSPy to evaluate and improve Datasette Agent's SQL system prompts

**Source:** Simon Willison  
**URL:** https://simonwillison.net/2026/Jul/2/dspy-datasette-agent-prompts/#atom-everything  
**Topics:** Evals / Benchmarks, AI Coding, Hands-on, Agents

Simon Willison demonstrated a practical workflow for using DSPy, the programmatic prompt optimization framework from Stanford, to evaluate and improve the system prompts powering Datasette Agent's SQL query feature. After attending an AI Engineering keynote on DSPy, Willison fired off an asynchronous research task in Claude Code for web using Claude Fable 5, asking it to install the latest Datasette alpha, datasette-agent, and dspy, then figure out how to use DSPy to evaluate and improve the main system prompts. The agent chose GPT 4.1 mini and nano as test models and identified several concrete directions for prompt improvement. The most notable finding concerned how schema information was presented: the prompt's schema listing included only table names while simultaneously advising the model not to call describe_table if it already had the information. This combination caused the model to guess column names, producing errors like page_count, o.order_id, and first_name, and falling into error-retry loops during baseline traces. The suggested fix was either including column names directly in the schema listing or softening the advice about avoiding describe_table calls. This represents a repeatable pattern for any developer maintaining system prompts for tool-using agents. Rather than manually reviewing traces and tweaking prompts by intuition, DSPy provides a structured way to identify where prompts mislead the model and measure whether changes actually improve performance. The approach is particularly relevant for agents that generate SQL or other structured queries, where small prompt ambiguities cascade into repeated failures that waste tokens and degrade user experience.

**Why it matters:** Systematic prompt optimization with DSPy surfaces failure modes that manual review misses, like schema presentation choices that cause column-name hallucination in tool-using agents. Developers maintaining agentic system prompts can apply this same workflow to identify and fix prompt ambiguities that waste tokens through error-retry loops.

---

## After spooking Trump into safety testing, Anthropic AI models get global release

**Source:** Ars Technica  
**URL:** https://arstechnica.com/tech-policy/2026/07/after-spooking-trump-into-safety-testing-anthropic-ai-models-get-global-release/  
**Topics:** Claude / Anthropic, Policy / Regulation, Industry

The US Department of Commerce has lifted export controls on Anthropic's Claude Fable 5 and Mythos 5 models, ending a roughly three-week period during which the models were unavailable outside the United States. Fable 5 is now available globally, and US organizations regained access to Mythos 5 on June 26. Anthropic is working with the government to expand Mythos access to additional domestic and international partners through the Glasswing program, which provides cybersecurity researchers at trusted companies access for defensive purposes. In a letter viewed by Reuters and The New York Times, Commerce Secretary Howard Lutnick stated that Anthropic would no longer need a license for exports or in-country transfers of the models, acknowledging that the company had taken steps in coordination with the government to address the identified risks. In exchange for the lifted restrictions, Anthropic agreed to expand its government partnership, established a program for external hackers to red-team its models, and created a dedicated internal team to monitor emerging jailbreak threats around the clock. The original shutdown order came on June 12, driven by concerns that adversarial nations could exploit the models to attack US infrastructure including the electric grid and banking systems. Anthropic shut down all access globally because it lacked the technical capability to block users by country at the time. Mythos in particular was characterized as uniquely attractive to malicious actors seeking to conduct cyberattacks. The Commerce Department retains the right to reimpose controls at any point, but for now both models are accessible through Claude platforms with cloud provider availability following.

**Why it matters:** Global availability of Fable 5 restores access to one of the most capable Claude models for developers outside the US who lost API access on June 12. Watch for cloud provider re-enablement timelines on AWS, Google Cloud, and Microsoft Foundry if your production stack depends on those endpoints.

---

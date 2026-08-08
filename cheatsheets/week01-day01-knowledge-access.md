# Five-Minute Cheat Sheet — Knowledge Access

Start with: What fact is needed, where is it authoritative, how fresh must it be, and what is the consequence of being wrong?

Use direct context when scope is small and known.
Use SQL/API for changing authoritative structured facts.
Use lexical search for exact terms/codes.
Use semantic retrieval when paraphrase/concept matching is needed.
Use hybrid only when measured query behavior justifies both.

Never assume a vector database is the enterprise AI system of record.

Key lesson: response latency, freshness, and correctness are different dimensions.

Interview framing:
> I would not start with RAG. I would classify the question, identify the source of truth, define freshness/correctness/latency requirements, then choose the simplest access path.

# Report

## Finding

The interesting technical failure mode is a public package registry and its documentation builder becoming an unexpected route for network access and output. RubyHack reconstructs this from uploaded gems. RubyGems confirms the broader abuse campaign but cannot verify whether AI agents created or published the packages.

## Client relevance

Agent permissions need to cover every external write path, including package publishing, and every automatic build triggered by those writes. Review credential reach and network access inside build services; do not assume public registry writes are inert.

## Limits

The report's author attribution remains disputed. RubyGems reports no evidence that attempted API key theft succeeded. Avoid definitive claims about OpenAI's role in specific payloads or successful credential theft.

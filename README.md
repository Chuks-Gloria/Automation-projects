Lead Generation & Personalisation Pipeline

Problem
A marketing agency needed a repeatable way to generate 
enriched local business leads for any niche and location 
without manual research. Each lead also needed a 
personalised outreach hook to improve reply rates.

Tools Used
N8N · Apify Google Maps Scraper · OpenAI GPT-4 · Google Sheets

How It Works
1. Schedule trigger fires daily
2. HTTP request sends search query to Apify Google 
   Maps Scraper and returns 20 local business leads
3. Filter node removes incomplete or low quality records
4. Loop Over Items processes each lead one at a time
5. Second HTTP request enriches each lead with contact 
   details from Apify
6. OpenAI generates a unique 1-sentence personalisation 
   hook per business based on name, niche and location
7. Aggregate collects all enriched leads
8. Google Sheets appends one row per lead

Output Columns
Business Name · Phone · Email · Website · City · 
Niche · Personalised Hook

Edge Cases Handled
- Empty email and phone fields: mapped directly from 
  Apify arrays using index [0] to avoid undefined errors
- API output path changes: diagnosed silent failures 
  by checking execution output at each node individually
- OpenAI output mapping: referenced previous node 
  explicitly using $('Message a model').item.json.output
  [0].content[0].text to reach the correct nested path

What Could Go Wrong
Connecting OpenAI before the loop causes it to run 
once on a single input instead of once per lead. 
Step order is critical and must follow: 
scrape → filter → loop → enrich → AI → aggregate → output

Result
20 enriched leads with personalised hooks generated 
and written to Google Sheets automatically with 
zero manual research required.

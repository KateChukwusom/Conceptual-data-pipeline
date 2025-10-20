# Conceptual-data-pipeline
This is a repository showing how a data pipeline is built to deliver solutions CONCEPTUALLY considering business requirements and tradeoffs, a project task by DE Mentorship Program, to help with understanding the fundamentals of Data Engineering, taught by Snr. Data Engineeer Najeeb Sulaiman.

# Problem Statement
As a Data Engineer at a telecom company called Beejan Technologies. Every day, thousands of customers complain about issues like poor network, incorrect billing, or bad customer service. These complaints come through different channels: social media, call center log files, SMS, and website forms.

The management is frustrated. Data is stored in different formats. The reporting team manually compiles spreadsheets. No single pipeline exists for this data flow. Reports are delayed. Teams work in silos.

We are to design a solution to bring all this data together, clean it, enrich it and make it ready to provide actionable insights. The first step is do map out a conceptual pipeline, first considering business requirements and of course what we will tradeoff to achieve highly efficient solutions specific to the problem.

# Business Requirements(From the business stakeholders POV:)
- "We want to see problems as soon as they happen"
- "We need accurate, consistent reports to make decisions."
- "We want it fast and affordable to run at scale"

# Tradeoffs
We are building this pipeline achieving low latency, high performance and low cost, trading one for another, achieving two at most or balancing them all.

## Data Sources
-> Social Media: Unstructured text, frequency- continuous / Real-time

-> Call Center Logs: Semi-structured, frequency - every few minutes or hourly.

-> SMS: Small structured messages, frequency - daily or hourly batch — depending on call volume.

-> Website Forms: Structured form data, frequency - hourly or daily batch — submissions accumulate before export.

## Data Ingestion

-> Tradeoff: Real-time ingestion from all channels would raise cost. Instead of streaming everything (high cost), a hybrid model balances low cost + at least moderate latency.

-> Decision: Streaming for real-time channels (social media, SMS); batch for slower sources (logs, forms).

## Data Processing/Transformation: 

-> Tradeoff: We trade off low latency for higher data quality and performance — ensuring that while insights may arrive a few minutes later, they are accurate, interpretable, and reliable enough for management decisions.

-> Decision:;
-> Raw data: All complaints (texts, messages, logs) arrive into the processing layer tagged with metadata: source, timestamp, language, region, etc.

-> Data Cleaning & Standardization: Remove duplicates and spam, Standardize timestamps and identifiers

-> Text Categorization: Apply rule-based + pattern recognition for known complaint keywords e.g., “network”, “signal”, “coverage” → Network Issue; “bill”, “charge”, “refund” → Billing Error.

-> Quality Check: Flag ambiguous or “unclassified” complaints for manual review later.

## Data Storage

-> Tradeoff: We trade off low latency as there will be slight delay in clean data availability.

-> Decision: We use data lake for all raw historical data and data warehouse for clean curated tables.

## Data Serving

-> Tradeoff: We tradeoff low cost as data warehouses(which is expensive) give low-latency query responses for dashboards and reports.

-> Decision: Operational dashboards which uses summarized, fast-updating data (low latency).

## Data Orchestration and Monitoring

-> Tradeoff: We trade off low cost for speed and fastness of failure detection.

-> Decision:;

-> Streaming sources (social media and SMS) -> processes near real-time.

-> Batch sources (call logs and web forms) -> Processes daily or hourly.

---> For failure to be detected; Each pipeline task reports success or failure after execution.

---> Send notifications via Slack, or email for failed jobs or data anomalies. Should include detailed error CONTEXT to speed up troubleshooting.

---> To ensure monitoring, every job/task logs start time, end time, success/failure, and error messages.

## DataOps
-> Tradeoff: We tradeoff low cost for fast processing and reliability which increases infrastructure costs.

-> Decision:;

->  We are running the pipeline in a cloud environment.

-> Use containerization or orchestration platforms to ensure reproducibility and easy deployment.

-> Deploy through version-controlled workflows with CI/CD for updates.

## Conclusion
Ensuring the business requirements are met, we consider balancing low latency, high performance and low optimized, cost while designing the conceptual data pipeline for Beejan Technologies. 

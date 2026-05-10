---
aliases:
---
## Key Points

1. **Bronze preserves raw data for reprocessing**: When a downstream transformation introduces a bug that corrupts silver tables, having untouched bronze data lets you replay ingestion from scratch without going back to source systems that may have already rotated their logs.
    
2. **Never transform at the bronze layer**: A team that applies currency conversion at ingestion later discovers exchange rates were stale; because the raw values were overwritten, there is no way to recompute with corrected rates. Keeping bronze pristine avoids this class of irreversible error.
    
3. **Medallion architecture is a data contract**: Each layer boundary acts as a versioned interface. Producers write to bronze, consumers read from gold, and the silver layer absorbs all schema evolution so neither side breaks when the other changes.
    
4. **Layers decouple ingestion from consumption**: An upstream API switches from XML to JSON. Because ingestion and analytics are separated by layer boundaries, only the bronze ingestion job needs updating while every downstream dashboard continues to operate normally.
    
5. **Metadata enrichment enables debugging**: Stamping every bronze record with ingestion timestamp, source file name, and pipeline run ID lets you trace a suspicious gold-layer aggregate all the way back to the exact file and minute it arrived, turning a multi-day investigation into a single query.
    
6. **Format-agnostic ingestion with Auto Loader**: A data lake receives Parquet from one vendor, CSV from another, and JSON from a third. Auto Loader's schema inference and file notification mode let a single bronze job ingest all three without writing format-specific parsers.
    
7. **Delta Lake provides time travel and versioning**: After an accidental DELETE wipes a partition, RESTORE TABLE AS OF VERSION recovers the data in seconds rather than requiring a full re-ingestion from source, which could take hours and consume API rate limits.
    
8. **Bronze tables grow append-only**: Append-only writes guarantee that late-arriving records never overwrite earlier ones, which is critical in regulated industries where auditors need proof that no historical record was silently modified.
    
9. **Bronze is the single source of truth**: When three different teams build their own silver tables with conflicting business logic, all of them can independently validate their results against the same bronze data, eliminating disputes about whose source is authoritative.

## Key Points

1. **Profiling first then cleaning**: Running summary statistics, null-rate analysis, and value distribution histograms before writing any transformation logic prevents you from building cleaning rules based on assumptions that the data does not actually support.
    
2. **Exploration reveals hidden data issues**: A column labeled "age" turns out to contain negative values and dates encoded as integers. Without exploratory profiling, a pipeline would silently propagate these values into silver, where they corrupt downstream aggregates for months before anyone notices.
    
3. **Deduplication is rarely straightforward**: Two records may share the same primary key but carry different timestamps, update flags, or source systems. Deciding which record to keep requires business context, not just a generic DISTINCT or ROW_NUMBER() call.
    
4. **Silver enforces type safety and schema**: Casting a string column to a proper timestamp or decimal type at the silver layer means every downstream consumer inherits correct types automatically, eliminating an entire category of bugs where dashboards silently compare strings instead of dates.
    
5. **Expectations bridge bronze to silver**: Databricks expectations let you declare constraints like "age must be between 0 and 120" directly in the pipeline. Records that violate expectations are quarantined rather than silently dropped, giving the team visibility into data quality without halting production.
    
6. **Row count comparison catches silent drops**: After every bronze-to-silver transformation, comparing input row count against output row count plus quarantined row count must balance to zero. A discrepancy signals a bug in the transformation logic, such as an unintended filter or a JOIN that fans out rows.
    
7. **Quality is a spectrum not a binary**: Some records are fully clean, some are usable with caveats, and some must be rejected. Tagging records with a quality score or confidence flag at the silver layer lets gold consumers decide their own tolerance threshold rather than forcing a single pass/fail decision for everyone.
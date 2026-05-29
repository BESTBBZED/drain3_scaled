# drain3_scaled

A modified version of [Drain3](https://github.com/logpai/Drain3) adapted for multi-parser, production-scale log template mining.

## Origin

This project is based on **Drain3** by [LogPAI](https://github.com/logpai/Drain3) (last updated July 2022). Drain3 is an online log template miner based on the [Drain](https://jiemingzhu.github.io/pub/pjhe_icws2017.pdf) algorithm, supporting streaming log parsing with persistence and masking.

> Original repository: https://github.com/logpai/Drain3
> License: MIT (IBM Corp. / LogPAI)

## Modifications from Original

The following changes were made to adapt Drain3 for production use in a multi-parser log analysis system:

### DIST-7514: Micro refactor for CBD3 adaptation
- Refactored source code structure to integrate with the CBD3 platform

### DIST-7587: Store trace level for event templates
- Extended template storage to persist trace/severity level alongside each mined template

### DIST-8270: Remove hardcoded field names
- Generalized functions that previously assumed specific field names, making the parser configurable for different log formats

### Additional changes
- Added `pyproject.toml` for modern Python packaging
- Added `LICENSE.txt`
- Extended `persistence_handler.py` with additional persistence capabilities
- Enhanced `template_miner.py` with production-scale improvements

## Project Structure

```
drain3/
├── drain.py                     # Core Drain algorithm (tree-based log parsing)
├── template_miner.py            # Wrapper with persistence + masking
├── template_miner_config.py     # Configuration management
├── masking.py                   # Log masking (IPs, numbers, etc.)
├── persistence_handler.py       # Abstract persistence + implementations
├── file_persistence.py          # File-based state persistence
├── kafka_persistence.py         # Kafka-based persistence
├── redis_persistence.py         # Redis-based persistence
├── memory_buffer_persistence.py # In-memory persistence
├── jaccard_drain.py             # Jaccard similarity variant
├── simple_profiler.py           # Performance profiling utilities
└── pyproject.toml               # Package metadata
```

## Usage

```python
from drain3.template_miner import TemplateMiner
from drain3.template_miner_config import TemplateMinerConfig

config = TemplateMinerConfig()
miner = TemplateMiner(config=config)

result = miner.add_log_message("Connected to host 10.0.0.1 on port 8080")
print(result["template_mined"])
# "Connected to host <*> on port <*>"
```

## License

MIT — see [LICENSE](LICENSE) and [drain3/LICENSE.txt](drain3/LICENSE.txt).

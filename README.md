# Setup

```bash
pip install watchdog git+https://github.com/huggingface/doc-builder.git
```

# Build Documentation

```bash
doc-builder build leaderboard docs/source/en --build_dir build_dir --not_python_module
```

# Preview Documentation

```bash
doc-builder preview leaderboard docs/source/en/ --not_python_module
```

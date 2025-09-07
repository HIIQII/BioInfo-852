hi i am Yue Shi BME Master student 
Assignment Week1

Code starts here:

### Samtool
```bash
samtools --version
```
### Command to create nested structure 
```bash
mkdir -p project/docs project/src/utils project/tests
```

### Command create different files in different dictionary
```bash
echo "print('Hello World')" > project/src/main.py
echo "# Utility functions" > project/src/utils/helpers.py
echo "# Documentation" > project/docs/readme.md
echo "# Test cases" > project/tests/test_main.py
```
### Access these files use relative/absolute Path
```bash
cat project/src/main.py
cat ~/Bioinfo852/BioInfo-852/project/src/main.py
```


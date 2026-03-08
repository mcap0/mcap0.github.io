---
title: Diffbackup.py - uno script per il backup differenziale
date: 2023-02-26 00:00:00 -500
categories: [Python]
tags: [backup, script]
image_path: /assets/
--- 

Utilizzo:
```bash
python3 diffbackup.py /cartellaorigine /cartelladestinazione
```


```python
import os
import hashlib
import shutil
import argparse


def hash_file(file_path):
    """
    Calcola l'hash SHA-256 del file specificato
    """
    hash_object = hashlib.sha256()
    try:
        with open(file_path, 'rb') as f:   
            for chunk in iter(lambda: f.read(4096), b""):
                hash_object.update(chunk)
        return hash_object.digest()
    except IsADirectoryError:
        return None

def backup_files(src_dir, dst_dir):
    """
    Copia tutti i file nella directory di origine nella directory di destinazione
    Se un file è già presente nella directory di destinazione e ha lo stesso hash, non viene copiato
    """
    # crea la directory di destinazione se non esiste
    os.makedirs(dst_dir, exist_ok=True)

    # copia tutti i file nella directory di origine
    for root, dirs, files in os.walk(src_dir):
        for filename in files:
            src_path = os.path.join(root, filename)
            dst_path = os.path.join(dst_dir, os.path.relpath(src_path, src_dir))

            # controlla se il file è stato modificato
            src_hash = hash_file(src_path)
            if os.path.exists(dst_path):
                dst_hash = hash_file(dst_path)
                if src_hash == dst_hash:
                   # print(f"Skipping {src_path} (already exists and has not been modified)")
                    continue

            # copia il file nella directory di destinazione
            os.makedirs(os.path.dirname(dst_path), exist_ok=True)
            shutil.copy2(src_path, dst_path)
            print(f"Copied {src_path} to {dst_path}")

def main():
    # Parsing degli argomenti da terminale
    parser = argparse.ArgumentParser(description='Effettua la copia dei file da una directory di origine ad una di destinazione')
    parser.add_argument('origine', metavar='DIR_ORIGINE', type=str, help='Percorso della directory di origine')
    parser.add_argument('destinazione', metavar='DIR_DESTINAZIONE', type=str, help='Percorso della directory di destinazione')
    args = parser.parse_args()

    # Verifica che la directory di origine esista
    if not os.path.exists(args.origine):
        print(f"Errore: La directory '{args.origine}' non esiste")
        return

    # Verifica che la directory di destinazione esista o la crea se non esiste
    if not os.path.exists(args.destinazione):
        os.makedirs(args.destinazione)

    # Effettua la copia dei file
    backup_files(args.origine, args.destinazione)

if __name__ == '__main__':
    main()

```
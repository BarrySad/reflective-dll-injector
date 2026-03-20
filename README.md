# Reflective dll injector

A reflective DLL injector written in C++/Assembly that loads a DLL into a 
remote process entirely from memory, without disk writes or LoadLibrary calls.
<br>

## How It Works

Traditional DLL injection relies on writing a DLL path to disk and calling 
LoadLibrary in the target process, which is heavily monitored by EDR solutions. 
Reflective injection instead maps the DLL directly into memory by implementing 
a custom loader within the DLL itself, bypassing the Windows loader entirely.

This implementation includes several evasion enhancements:

- **Direct syscalls via SysWhispers3** — rather than calling Windows API 
  functions through ntdll.dll (where EDR hooks are typically placed), syscalls 
  are invoked directly using x64 assembly stubs, bypassing 
  userland hooks entirely
- **Encrypted payload staging** — the target DLL is XOR-encrypted at rest and 
  decrypted in memory immediately before injection, avoiding static signature 
  detection
- **Garbage code generation** — junk instructions are interspersed throughout 
  the binary to disrupt heuristic analysis and inflate entropy in ways that 
  confuse sandbox classifiers

## Detection

Currently **4/72** detections on VirusTotal when last checked.
<img width="1484" height="1100" alt="virus-total-dll-loader" src="https://github.com/user-attachments/assets/e5abccf1-14b7-48a4-a510-dfed380f25a5" />

## Disclaimer

This tool was developed for authorized security research and educational 
purposes only. The author is not responsible for any misuse. Only use against 
systems you own or have explicit written permission to test.

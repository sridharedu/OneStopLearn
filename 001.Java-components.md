```markdown
🧰 **JDK (Java Development Kit)**  
- → First thing to install for Java development  
- → Includes: `javac` (🛠 compiler), `java` (🚀 launcher), 📦 JRE, and other tools  
- → OS-specific builds available (Windows/Linux/Mac)  
- → Needed for both compiling and running Java apps  

🔁 **JRE (Java Runtime Environment)**  
- → Subset of JDK  
- → Includes: JVM and runtime libraries  
- → ⚙️ Used for running only (not compiling)  
- → 📦 Usually bundled with JDK  

🧠 **JVM (Java Virtual Machine)**  
- → 🚀 Launched using `java ClassName`  
- → Reads `.class` (bytecode) → converts to machine code  
- → 🌍 Enables platform independence: compile on Windows, run on Linux  
- → JVM is OS-specific, but bytecode is portable  

🛠 **javac (Java Compiler)**  
- → Converts `.java` → `.class`  
- → 📦 Output is bytecode (not native machine code)  
- → 🌐 Portable across platforms  

🚀 **java (Java Command)**  
- → Used to run `.class` files  
- → Launches JVM → interprets or JIT-compiles the bytecode  

📦 **Bytecode**  
- → Output of compilation  
- → Stored in `.class` files  
- → Platform-independent, readable only by JVM  

⚡ **JIT Compiler (Just-In-Time)**  
- → Built into JVM  
- → Converts bytecode → machine code at runtime  
- → 📈 Improves performance using inlining, caching, etc.  
- → ✅ Enabled by default  

🚫 **Disabling JIT (for debugging)**  
- → 🔧 Use `-Djava.compiler=NONE` with the `java` command  
- → 🐢 JVM runs in interpreter-only mode (no JIT optimization)  

📌 **Execution Flow**  
- → 1️⃣ Write `.java`  
- → 2️⃣ Compile using `javac` → generates `.class`  
- → 3️⃣ Run using `java` → starts JVM  
- → 4️⃣ JVM loads `.class` → JIT compiles → executes on OS
```

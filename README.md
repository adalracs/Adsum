# Lenguaje Adsum

El lenguaje de programación para los humanos. Diseñado desde su nacimiento para los nuevos paradigmas: Blockchain, IoT, Robótica y Sistemas Neuronales.

## ¿Por qué Adsum?

Los lenguajes actuales fueron diseñados para paradigmas del siglo XX, pero el mundo cambió y hoy desarrollamos sistemas descentralizados, dispositivos embebidos, arquitecturas neuronales y hardware heterogéneo. Adsum nace para esos nuevos paradigmas — no como adaptación, sino como fundamento.

## Filosofía

* **Multiidioma real en el código fuente.** El lenguaje debe ser comprensible, cercano al humano y accesible sin importar tu cultura, ubicación o país.
* **Tipado fuerte y semánticamente determinista.**
* **Generación directa de código máquina.** Sin capas de abstracción que desperdicien recursos.
* **Primitivas nativas para criptografía, blockchain, red e IA.** Como parte fundamental del núcleo, no como librerías.

## Estado del Proyecto

Versión actual: **1.9.0**

## Arquitectura del Compilador

Adsum es un compilador completo de extremo a extremo: código fuente en cualquier idioma humano → binario estático ejecutable en cuatro arquitecturas. Sin LLVM, GCC, generadores de parsers, ni librerías de terceros en runtime.

Pipeline de 10 fases:

```
Código fuente (.adsum, cualquier idioma humano)
  ↓
FASE 0  — Normalización multiidioma          
  ↓
FASE 1  — Preprocesamiento (@resource)       
  ↓
FASE 2  — Lexer + Parser → AST              
  ↓
FASE 3  — Carga del Contrato Formal          
  ↓
FASE 4  — Type Checking                      
  ↓
FASE 5  — Integrity Guard                    
  ↓
FASE 6  — Formal Guard                       
             ├── Separation Logic            
             └── Region Solver               
  ↓
FASE 7  — Frame Builder + Codegen ISA        
  ↓
FASE 8  — Ensamblado                         
  ↓
FASE 9  — Linking                            
  ↓
Binario estático ejecutable
```

## Targets de Compilación

El mismo código fuente compila a cualquier target sin modificar el programa:

| Formato  | Plataforma          |
|----------|---------------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 bare metal |
| ELF64 AArch64 | AArch64 Linux  |

## Multiidioma Real

Adsum soporta 15 idiomas de programación:

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## Soberanía Lingüística

Adsum no traduce documentación, traduce el lenguaje mismo. El código puede escribirse en la lengua materna del programador sin alterar la semántica ni la representación interna. El IR neutro actúa como contrato formal único, garantizando que todas las variantes lingüísticas compilen al mismo resultado binario, lo que permite:

* Independencia cultural en el desarrollo de software
* Acceso técnico sin dependencia del inglés
* Uniformidad semántica entre idiomas
* Escalabilidad hacia nuevos lenguajes humanos

## Contrato Formal (IR)

El `system_manifest.json` actúa como representación intermedia contractual del compilador. Es la fuente única de verdad que parametriza simultáneamente:

* **TypeChecker** — firmas de tipos de todos los builtins
* **SeparationLogicVerifier** — dominios de memoria y reglas cross-domain (CDRs)
* **CodeGen** — registros ABI y funciones `inject_*` por arquitectura

El `ManifestLoader` carga este contrato, valida 6 invariantes estrictas, y lo distribuye a los tres consumidores. Ningún módulo hardcodea firmas, dominios ni registros — todo se deriva del manifest.

## Soberanía Criptográfica

El stack criptográfico de Adsum reemplaza completamente libsodium y OpenSSL, implementando desde primeros principios:

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** sign, verify, keygen (RFC 8032)
* **ChaCha20** encrypt/decrypt (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** aritmética de campo finito en tiempo constante
* **Curva Ed25519** con Montgomery ladder branchless (CMOV)

Todas las operaciones criptográficas se emiten como código máquina nativo. La arquitectura spec/emit separa la especificación algorítmica del backend de emisión, habilitando soporte multi-arquitectura.

## Networking Nativo

Red TCP/IP y HTTP implementados directamente con syscalls del sistema operativo:

* **DNS** — Resolución nativa (UDP port 53), lee `/etc/resolv.conf`
* **TCP** — connect, send, recv, close sin libc
* **HTTP** — GET/POST con constructor de headers
* **Traceroute** — Trazado de ruta IP (ICMP TTL)
* **ERFR** — Protocolo de red propio con contexto de nodo

## IaG — Inteligencia Artificial Genuina

Modelo neuronal biológicamente inspirado que compila a instrucciones AND, POPCNT, SHR y CMP. Sin punto flotante, sin matrices, sin backpropagation.

* **Neurona** — 28 bits de axones, 4 células de Schwann con compuertas de 7 bits
* **Procesamiento** — Schwann filtra (AND) y convierte a frecuencia (POPCNT), dendrita compara contra umbral
* **Topología** — Triángulo de Pascal invertido con fases emergentes
* **Instintos** — Comportamientos precableados en JSON (defensivos, evaluativos, proactivos, sociales)
* **Memoria** — Sensorial (1 tick), corto plazo (16 snapshots), largo plazo (256 patrones con decaimiento)
* **Primitivas nativas** — axon_cargar, dendrita_disparar, udp_crear, json_a_texto disponibles como builtins del lenguaje

## Verificación Formal

Barrera matemática que garantiza seguridad de memoria sin garbage collector:

* **Separation Logic** — Demuestra que los dominios de memoria (SAFE_STACK, IO_VOLATILE, IA_SENSORY, CRYPTO_SECURE, IOT_REALTIME, QUANTUM) son disjuntos
* **Region Solver** — Análisis de escape estático: contención léxica, detección de punteros colgantes, prevención de escape de referencias locales
* **Integrity Guard** — Null integrity, mutabilidad explícita, control de bloques `peligroso`
* **Cross-Domain Rules** — CDRs que previenen contaminación entre dominios sin bloque `peligroso` explícito
* **ProgramProfile** — El FormalGuard produce un perfil sellado que viaja al CodeGen, conectando análisis estático con generación de código máquina

## Algoritmos Cuánticos como Builtins

Los algoritmos cuánticos son primitivas nativas del lenguaje — el compilador emite el circuito como código máquina:

* `grover(N)` — Algoritmo de búsqueda de Grover
* `qft(N)` — Transformada Cuántica de Fourier
* `shor(a, N)` — Algoritmo de factorización de Shor
* `deutsch_jozsa(N)` — Algoritmo Deutsch-Jozsa
* `bernstein_vazirani(N)` — Algoritmo Bernstein-Vazirani

Sin Qiskit. Sin dependencias externas.

## Dependencias en Runtime

**Ninguna.** El binario Adsum resultante no enlaza contra librerías de terceros:

* Sin libsodium
* Sin libcurl
* Sin libc
* Sin OpenSSL
* Sin Qiskit

El compilador en sí está escrito en Python y requiere Python 3.x para ejecutarse.

## Arquitecturas Disponibles

| Plataforma         | Estado          |
|--------------------|-----------------|
| x86_64 Linux       | Disponible v1.9.0 |
| x86_64 Windows     | Disponible v1.9.0 |
| ESP32-S3 bare metal | Disponible v1.9.0 |
| AArch64 Linux      | Disponible v1.9.0 |

## Licencia

SEGÚN EL ACUERDO DE LICENCIA DE USUARIO FINAL (EULA) — ADSUM 1.x

---

# Adsum 编程语言

为人类设计的编程语言。从诞生之初便面向新范式：区块链、物联网、机器人与神经系统。

## 为什么选择 Adsum？

现有编程语言是为二十世纪的范式而设计的，但世界已经改变——今天我们开发去中心化系统、嵌入式设备、神经架构和异构硬件。Adsum 为这些新范式而生——不是作为适配层，而是作为基础。

## 设计哲学

* **源代码中真正的多语言支持。** 语言应当易于理解，贴近人类，无论你的文化、地域或国家。
* **强类型且语义确定性。**
* **直接生成机器码。** 无浪费资源的抽象层。
* **密码学、区块链、网络与 AI 的原生原语。** 作为核心的根本部分，而非库。

## 项目状态

当前版本：**1.9.0**

## 编译器架构

Adsum 是一个完整的端到端编译器：任何人类语言的源代码 → 四种架构的静态可执行二进制文件。无 LLVM、无 GCC、无解析器生成器、无运行时第三方库。

10 阶段流水线：

```
源代码（.adsum，任何人类语言）
  ↓
阶段 0  — 多语言规范化          
  ↓
阶段 1  — 预处理（@resource）   
  ↓
阶段 2  — 词法 + 解析 → AST    
  ↓
阶段 3  — 加载形式合约          
  ↓
阶段 4  — 类型检查              
  ↓
阶段 5  — 完整性守卫            
  ↓
阶段 6  — 形式守卫              
             ├── 分离逻辑       
             └── 区域求解器     
  ↓
阶段 7  — 帧构建 + ISA 代码生成 
  ↓
阶段 8  — 汇编                  
  ↓
阶段 9  — 链接                  
  ↓
静态可执行二进制文件
```

## 编译目标

相同源代码无需修改即可编译至任意目标：

| 格式  | 平台          |
|-------|---------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 裸机 |
| ELF64 AArch64 | AArch64 Linux  |

## 真正的多语言

Adsum 支持 15 种编程语言：

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## 语言主权

Adsum 不翻译文档，而是翻译语言本身。代码可以用程序员的母语编写，而不改变语义或内部表示。中立 IR 作为唯一形式合约，保证所有语言变体编译为相同的二进制结果，从而实现：

* 软件开发中的文化独立性
* 无需依赖英语的技术访问
* 跨语言的语义统一性
* 向新人类语言的可扩展性

## 形式合约（IR）

`system_manifest.json` 作为编译器的合约中间表示。它是同时参数化以下组件的唯一真实来源：

* **TypeChecker** — 所有内置函数的类型签名
* **SeparationLogicVerifier** — 内存域与跨域规则（CDR）
* **CodeGen** — ABI 寄存器与各架构的 `inject_*` 函数

`ManifestLoader` 加载此合约，验证 6 个严格不变量，并将其分发给三个消费者。没有模块硬编码签名、域或寄存器——一切均从 manifest 派生。

## 密码学主权

Adsum 的密码学栈完全取代 libsodium 和 OpenSSL，从第一原理实现：

* **SHA-256 / SHA-512**（FIPS 180-4）
* **Ed25519** 签名、验证、密钥生成（RFC 8032）
* **ChaCha20** 加密/解密（RFC 8439）
* **HMAC-SHA256**（RFC 2104）
* **GF(2²⁵⁵−19)** 常数时间有限域算术
* **Ed25519 曲线**，带无分支 Montgomery ladder（CMOV）

所有密码学操作均以原生机器码发出。spec/emit 架构将算法规范与发出后端分离，支持多架构。

## 原生网络

TCP/IP 和 HTTP 网络直接通过操作系统系统调用实现：

* **DNS** — 原生解析（UDP 端口 53），读取 `/etc/resolv.conf`
* **TCP** — 无 libc 的 connect、send、recv、close
* **HTTP** — 带头部构建器的 GET/POST
* **Traceroute** — IP 路由跟踪（ICMP TTL）
* **ERFR** — 具有节点上下文的自有网络协议

## IaG — 真正的人工智能

仿生神经模型，编译为 AND、POPCNT、SHR 和 CMP 指令。无浮点，无矩阵，无反向传播。

* **神经元** — 28 位轴突，4 个带 7 位门控的施旺细胞
* **处理** — 施旺过滤（AND）并转换为频率（POPCNT），树突与阈值比较
* **拓扑** — 带涌现阶段的倒置帕斯卡三角形
* **本能** — JSON 中预配线的行为（防御性、评估性、主动性、社交性）
* **记忆** — 感觉（1 tick）、短期（16 快照）、长期（256 个带衰减的模式）
* **原生原语** — axon_cargar、dendrita_disparar、udp_crear、json_a_texto 作为语言内置函数可用

## 形式验证

无垃圾回收器保证内存安全的数学屏障：

* **分离逻辑** — 证明内存域（SAFE_STACK、IO_VOLATILE、IA_SENSORY、CRYPTO_SECURE、IOT_REALTIME、QUANTUM）互不相交
* **区域求解器** — 静态逃逸分析：词法包含、悬挂指针检测、防止局部引用逃逸
* **完整性守卫** — 空完整性、显式可变性、`peligroso` 块控制
* **跨域规则** — CDR 防止无显式 `peligroso` 块的跨域污染
* **ProgramProfile** — FormalGuard 产生密封配置文件，传递至 CodeGen，将静态分析与机器码生成连接

## 量子算法作为内置函数

量子算法是语言的原生原语——编译器将电路作为机器码发出：

* `grover(N)` — Grover 搜索算法
* `qft(N)` — 量子傅里叶变换
* `shor(a, N)` — Shor 因式分解算法
* `deutsch_jozsa(N)` — Deutsch-Jozsa 算法
* `bernstein_vazirani(N)` — Bernstein-Vazirani 算法

无 Qiskit。无外部依赖。

## 运行时依赖

**无。** 生成的 Adsum 二进制文件不链接任何第三方库：

* 无 libsodium
* 无 libcurl
* 无 libc
* 无 OpenSSL
* 无 Qiskit

编译器本身以 Python 编写，执行时需要 Python 3.x。

## 可用架构

| 平台         | 状态          |
|--------------|---------------|
| x86_64 Linux       | 可用 v1.9.0 |
| x86_64 Windows     | 可用 v1.9.0 |
| ESP32-S3 裸机 | 可用 v1.9.0 |
| AArch64 Linux      | 可用 v1.9.0 |

## 许可证

根据最终用户许可协议（EULA）— ADSUM 1.x

---

# Adsum प्रोग्रामिंग भाषा

मनुष्यों के लिए प्रोग्रामिंग भाषा। जन्म से ही नए प्रतिमानों के लिए डिज़ाइन: ब्लॉकचेन, IoT, रोबोटिक्स और न्यूरल सिस्टम।

## Adsum क्यों?

वर्तमान भाषाएँ बीसवीं सदी के प्रतिमानों के लिए डिज़ाइन की गई थीं, लेकिन दुनिया बदल गई है और आज हम विकेंद्रीकृत सिस्टम, एम्बेडेड डिवाइस, न्यूरल आर्किटेक्चर और हेटेरोजेनस हार्डवेयर विकसित करते हैं। Adsum इन नए प्रतिमानों के लिए जन्मा है — अनुकूलन के रूप में नहीं, बल्कि आधार के रूप में।

## दर्शन

* **सोर्स कोड में वास्तविक बहुभाषी।** भाषा समझने योग्य, मानव के करीब और आपकी संस्कृति, स्थान या देश की परवाह किए बिना सुलभ होनी चाहिए।
* **मजबूत टाइपिंग और शब्दार्थ रूप से निर्धारक।**
* **सीधे मशीन कोड जनरेशन।** संसाधन बर्बाद करने वाली अमूर्त परतें नहीं।
* **क्रिप्टोग्राफी, ब्लॉकचेन, नेटवर्क और AI के लिए नेटिव प्रिमिटिव।** कोर के मूलभूत भाग के रूप में, लाइब्रेरी के रूप में नहीं।

## परियोजना की स्थिति

वर्तमान संस्करण: **1.9.0**

## कंपाइलर आर्किटेक्चर

Adsum एक पूर्ण एंड-टू-एंड कंपाइलर है: किसी भी मानव भाषा में सोर्स कोड → चार आर्किटेक्चर में स्टैटिक एक्जीक्यूटेबल बाइनरी। बिना LLVM, GCC, पार्सर जेनरेटर, या रनटाइम में तीसरे पक्ष की लाइब्रेरी के।

10-चरण पाइपलाइन:

```
सोर्स कोड (.adsum, कोई भी मानव भाषा)
  ↓
चरण 0  — बहुभाषी सामान्यीकरण          
  ↓
चरण 1  — प्रीप्रोसेसिंग (@resource)   
  ↓
चरण 2  — Lexer + Parser → AST         
  ↓
चरण 3  — औपचारिक अनुबंध लोड करना    
  ↓
चरण 4  — टाइप चेकिंग                 
  ↓
चरण 5  — इंटीग्रिटी गार्ड            
  ↓
चरण 6  — फॉर्मल गार्ड               
             ├── सेपरेशन लॉजिक       
             └── रीजन सॉल्वर         
  ↓
चरण 7  — फ्रेम बिल्डर + कोडजेन ISA  
  ↓
चरण 8  — असेंबलिंग                   
  ↓
चरण 9  — लिंकिंग                     
  ↓
स्टैटिक एक्जीक्यूटेबल बाइनरी
```

## कंपाइलेशन टारगेट

वही सोर्स कोड प्रोग्राम को बदले बिना किसी भी टारगेट पर कंपाइल होता है:

| फॉर्मेट  | प्लेटफॉर्म          |
|----------|---------------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 बेयर मेटल |
| ELF64 AArch64 | AArch64 Linux  |

## वास्तविक बहुभाषी

Adsum 15 प्रोग्रामिंग भाषाओं का समर्थन करता है:

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## भाषाई संप्रभुता

Adsum दस्तावेज़ीकरण का अनुवाद नहीं करता, भाषा का ही अनुवाद करता है। कोड प्रोग्रामर की मातृभाषा में लिखा जा सकता है बिना शब्दार्थ या आंतरिक प्रतिनिधित्व को बदले। तटस्थ IR एकमात्र औपचारिक अनुबंध के रूप में कार्य करता है, यह सुनिश्चित करते हुए कि सभी भाषाई रूप एक ही बाइनरी परिणाम में कंपाइल हों, जो सक्षम बनाता है:

* सॉफ्टवेयर विकास में सांस्कृतिक स्वतंत्रता
* अंग्रेजी पर निर्भरता के बिना तकनीकी पहुँच
* भाषाओं के बीच शब्दार्थ एकरूपता
* नई मानव भाषाओं की ओर स्केलेबिलिटी

## औपचारिक अनुबंध (IR)

`system_manifest.json` कंपाइलर के संविदात्मक मध्यवर्ती प्रतिनिधित्व के रूप में कार्य करता है। यह एकमात्र सत्य स्रोत है जो एक साथ पैरामीटर करता है:

* **TypeChecker** — सभी बिल्टइन के टाइप सिग्नेचर
* **SeparationLogicVerifier** — मेमोरी डोमेन और क्रॉस-डोमेन नियम (CDRs)
* **CodeGen** — ABI रजिस्टर और आर्किटेक्चर द्वारा `inject_*` फ़ंक्शन

`ManifestLoader` इस अनुबंध को लोड करता है, 6 सख्त इनवेरिएंट को मान्य करता है, और इसे तीन उपभोक्ताओं को वितरित करता है। कोई भी मॉड्यूल सिग्नेचर, डोमेन या रजिस्टर हार्डकोड नहीं करता — सब कुछ manifest से व्युत्पन्न होता है।

## क्रिप्टोग्राफिक संप्रभुता

Adsum का क्रिप्टोग्राफिक स्टैक पूरी तरह से libsodium और OpenSSL को प्रतिस्थापित करता है, प्रथम सिद्धांतों से लागू करता है:

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** साइन, वेरिफाई, कीजेन (RFC 8032)
* **ChaCha20** एन्क्रिप्ट/डिक्रिप्ट (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** स्थिर समय परिमित क्षेत्र अंकगणित
* **Ed25519 कर्व** ब्रांचलेस Montgomery ladder (CMOV) के साथ

सभी क्रिप्टोग्राफिक ऑपरेशन नेटिव मशीन कोड के रूप में उत्सर्जित होते हैं। spec/emit आर्किटेक्चर एल्गोरिदमिक विनिर्देश को उत्सर्जन बैकएंड से अलग करता है, मल्टी-आर्किटेक्चर समर्थन सक्षम करता है।

## नेटिव नेटवर्किंग

TCP/IP और HTTP नेटवर्क ऑपरेटिंग सिस्टम syscalls के साथ सीधे लागू:

* **DNS** — नेटिव रिज़ॉल्यूशन (UDP पोर्ट 53), `/etc/resolv.conf` पढ़ता है
* **TCP** — libc के बिना connect, send, recv, close
* **HTTP** — हेडर बिल्डर के साथ GET/POST
* **Traceroute** — IP रूट ट्रेसिंग (ICMP TTL)
* **ERFR** — नोड संदर्भ के साथ स्वयं का नेटवर्क प्रोटोकॉल

## IaG — वास्तविक कृत्रिम बुद्धिमत्ता

जैविक रूप से प्रेरित न्यूरल मॉडल जो AND, POPCNT, SHR और CMP निर्देशों में कंपाइल होता है। बिना फ्लोटिंग पॉइंट, बिना मैट्रिक्स, बिना बैकप्रोपेगेशन।

* **न्यूरॉन** — 28-बिट एक्सॉन, 7-बिट गेट के साथ 4 श्वान कोशिकाएं
* **प्रसंस्करण** — श्वान फ़िल्टर (AND) और आवृत्ति में परिवर्तित (POPCNT), डेंड्राइट थ्रेशोल्ड से तुलना
* **टोपोलॉजी** — उभरती हुई चरणों के साथ उल्टा पास्कल त्रिभुज
* **प्रवृत्तियाँ** — JSON में पूर्व-वायर्ड व्यवहार (रक्षात्मक, मूल्यांकनात्मक, सक्रिय, सामाजिक)
* **स्मृति** — संवेदी (1 टिक), अल्पकालिक (16 स्नैपशॉट), दीर्घकालिक (क्षय के साथ 256 पैटर्न)
* **नेटिव प्रिमिटिव** — axon_cargar, dendrita_disparar, udp_crear, json_a_texto भाषा के बिल्टइन के रूप में उपलब्ध

## औपचारिक सत्यापन

गार्बेज कलेक्टर के बिना मेमोरी सुरक्षा की गारंटी देने वाली गणितीय बाधा:

* **सेपरेशन लॉजिक** — सिद्ध करता है कि मेमोरी डोमेन (SAFE_STACK, IO_VOLATILE, IA_SENSORY, CRYPTO_SECURE, IOT_REALTIME, QUANTUM) विसंयुक्त हैं
* **रीजन सॉल्वर** — स्टैटिक एस्केप विश्लेषण: शाब्दिक संयोजन, डैंगलिंग पॉइंटर डिटेक्शन, स्थानीय संदर्भों के एस्केप की रोकथाम
* **इंटीग्रिटी गार्ड** — नल इंटीग्रिटी, स्पष्ट म्युटेबिलिटी, `peligroso` ब्लॉक नियंत्रण
* **क्रॉस-डोमेन नियम** — CDRs जो स्पष्ट `peligroso` ब्लॉक के बिना डोमेन के बीच संदूषण को रोकते हैं
* **ProgramProfile** — FormalGuard एक सीलबंद प्रोफाइल बनाता है जो CodeGen तक जाता है, स्टैटिक विश्लेषण को मशीन कोड जनरेशन से जोड़ता है

## बिल्टइन के रूप में क्वांटम एल्गोरिदम

क्वांटम एल्गोरिदम भाषा के नेटिव प्रिमिटिव हैं — कंपाइलर सर्किट को मशीन कोड के रूप में उत्सर्जित करता है:

* `grover(N)` — Grover खोज एल्गोरिदम
* `qft(N)` — क्वांटम फूरियर ट्रांसफॉर्म
* `shor(a, N)` — Shor फैक्टराइज़ेशन एल्गोरिदम
* `deutsch_jozsa(N)` — Deutsch-Jozsa एल्गोरिदम
* `bernstein_vazirani(N)` — Bernstein-Vazirani एल्गोरिदम

बिना Qiskit। बिना बाहरी निर्भरता।

## रनटाइम निर्भरताएं

**कोई नहीं।** परिणामी Adsum बाइनरी किसी भी तृतीय पक्ष लाइब्रेरी से लिंक नहीं होती:

* बिना libsodium
* बिना libcurl
* बिना libc
* बिना OpenSSL
* बिना Qiskit

कंपाइलर स्वयं Python में लिखा है और चलाने के लिए Python 3.x की आवश्यकता है।

## उपलब्ध आर्किटेक्चर

| प्लेटफॉर्म         | स्थिति          |
|--------------------|-----------------|
| x86_64 Linux       | उपलब्ध v1.9.0 |
| x86_64 Windows     | उपलब्ध v1.9.0 |
| ESP32-S3 बेयर मेटल | उपलब्ध v1.9.0 |
| AArch64 Linux      | उपलब्ध v1.9.0 |

## लाइसेंस

अंतिम उपयोगकर्ता लाइसेंस समझौते (EULA) के अनुसार — ADSUM 1.x

---

# لغة Adsum البرمجية

لغة البرمجة للبشر. مصممة منذ نشأتها للنماذج الجديدة: البلوك تشين، وإنترنت الأشياء، والروبوتيك، والأنظمة العصبية.

## لماذا Adsum؟

صُممت اللغات الحالية لنماذج القرن العشرين، لكن العالم تغير واليوم نطور أنظمة لامركزية وأجهزة مدمجة ومعماريات عصبية وأجهزة غير متجانسة. وُلدت Adsum لهذه النماذج الجديدة — ليس كتكيف، بل كأساس.

## الفلسفة

* **تعدد لغوي حقيقي في الكود المصدري.** يجب أن تكون اللغة مفهومة، قريبة من الإنسان وسهلة الوصول بغض النظر عن ثقافتك أو موقعك أو بلدك.
* **كتابة قوية ومحددة دلالياً.**
* **توليد مباشر لكود الآلة.** بدون طبقات تجريد تهدر الموارد.
* **بدائل أصلية للتشفير والبلوك تشين والشبكة والذكاء الاصطناعي.** كجزء أساسي من النواة، وليس كمكتبات.

## حالة المشروع

الإصدار الحالي: **1.9.0**

## معمارية المترجم

Adsum هو مترجم كامل من طرف إلى طرف: كود مصدري بأي لغة بشرية → ملف ثنائي قابل للتنفيذ بشكل ثابت على أربع معماريات. بدون LLVM أو GCC أو مولدات المحللات أو مكتبات طرف ثالث في وقت التشغيل.

خط أنابيب من 10 مراحل:

```
الكود المصدري (.adsum، أي لغة بشرية)
  ↓
المرحلة 0  — التطبيع متعدد اللغات          
  ↓
المرحلة 1  — المعالجة المسبقة (@resource)  
  ↓
المرحلة 2  — Lexer + Parser → AST          
  ↓
المرحلة 3  — تحميل العقد الرسمي            
  ↓
المرحلة 4  — فحص الأنواع                   
  ↓
المرحلة 5  — حارس النزاهة                  
  ↓
المرحلة 6  — الحارس الرسمي                 
             ├── منطق الفصل               
             └── محلل المناطق             
  ↓
المرحلة 7  — باني الإطار + كودجن ISA       
  ↓
المرحلة 8  — التجميع                        
  ↓
المرحلة 9  — الربط                          
  ↓
ملف ثنائي ثابت قابل للتنفيذ
```

## أهداف التجميع

يُجمَّل نفس الكود المصدري لأي هدف دون تعديل البرنامج:

| التنسيق  | المنصة          |
|----------|-----------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 معدن مجرد |
| ELF64 AArch64 | AArch64 Linux  |

## تعدد لغوي حقيقي

يدعم Adsum 15 لغة برمجة:

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## السيادة اللغوية

لا تترجم Adsum الوثائق، بل تترجم اللغة نفسها. يمكن كتابة الكود بلغة المبرمج الأم دون تغيير الدلالات أو التمثيل الداخلي. يعمل IR المحايد كعقد رسمي وحيد، مما يضمن أن جميع المتغيرات اللغوية تُجمَّل إلى نفس النتيجة الثنائية، مما يتيح:

* الاستقلالية الثقافية في تطوير البرمجيات
* الوصول التقني دون الاعتماد على الإنجليزية
* التوحيد الدلالي بين اللغات
* قابلية التوسع نحو لغات بشرية جديدة

## العقد الرسمي (IR)

يعمل `system_manifest.json` كتمثيل وسيط تعاقدي للمترجم. إنه مصدر الحقيقة الوحيد الذي يُحدد في آنٍ واحد:

* **TypeChecker** — توقيعات أنواع جميع الوظائف المدمجة
* **SeparationLogicVerifier** — نطاقات الذاكرة وقواعد عبر النطاقات (CDRs)
* **CodeGen** — سجلات ABI ووظائف `inject_*` لكل معمارية

يُحمِّل `ManifestLoader` هذا العقد، ويتحقق من 6 ثوابت صارمة، ويوزعه على المستهلكين الثلاثة. لا يُشفِّر أي وحدة التوقيعات أو النطاقات أو السجلات بشكل ثابت — كل شيء مشتق من المانيفست.

## السيادة التشفيرية

يُحل مكدس التشفير في Adsum محل libsodium وOpenSSL بالكامل، ويُنفذ من المبادئ الأولى:

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** التوقيع والتحقق وتوليد المفاتيح (RFC 8032)
* **ChaCha20** التشفير/فك التشفير (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** حساب الحقل المنتهي بوقت ثابت
* **منحنى Ed25519** مع Montgomery ladder بلا تفرع (CMOV)

تُصدر جميع عمليات التشفير ككود آلة أصلي. تفصل معمارية spec/emit المواصفة الخوارزمية عن خلفية الإصدار، مما يتيح دعم متعدد المعماريات.

## الشبكات الأصلية

شبكة TCP/IP وHTTP مُنفَّذة مباشرةً باستدعاءات نظام التشغيل:

* **DNS** — الحل الأصلي (UDP منفذ 53)، يقرأ `/etc/resolv.conf`
* **TCP** — connect وsend وrecv وclose بدون libc
* **HTTP** — GET/POST مع منشئ الرؤوس
* **Traceroute** — تتبع مسار IP (ICMP TTL)
* **ERFR** — بروتوكول شبكة خاص مع سياق العقدة

## IaG — الذكاء الاصطناعي الحقيقي

نموذج عصبي مستوحى بيولوجياً يُجمَّل إلى تعليمات AND وPOPCNT وSHR وCMP. بدون فاصلة عائمة، بدون مصفوفات، بدون الانتشار العكسي.

* **الخلية العصبية** — 28 بت من المحاور، 4 خلايا شوان ببوابات 7 بت
* **المعالجة** — شوان يُصفِّي (AND) ويُحوِّل إلى تردد (POPCNT)، التغصنات تُقارن مع العتبة
* **الطوبولوجيا** — مثلث باسكال المقلوب بمراحل ناشئة
* **الغرائز** — سلوكيات مُبرمجة مسبقاً في JSON (دفاعية وتقييمية وإيجابية واجتماعية)
* **الذاكرة** — حسية (1 نبضة)، قصيرة المدى (16 لقطة)، طويلة المدى (256 نمطاً مع تلاشٍ)
* **البدائل الأصلية** — axon_cargar وdendrita_disparar وudp_crear وjson_a_texto متاحة كوظائف مدمجة في اللغة

## التحقق الرسمي

حاجز رياضي يضمن أمان الذاكرة بدون جامع مهملات:

* **منطق الفصل** — يُثبت أن نطاقات الذاكرة (SAFE_STACK وIO_VOLATILE وIA_SENSORY وCRYPTO_SECURE وIOT_REALTIME وQUANTUM) متقاطعة
* **محلل المناطق** — تحليل الهروب الثابت: الاحتواء المعجمي، والكشف عن المؤشرات المعلقة، ومنع هروب المراجع المحلية
* **حارس النزاهة** — نزاهة null، وقابلية التحول الصريحة، والتحكم في كتل `peligroso`
* **قواعد عبر النطاقات** — CDRs التي تمنع التلوث بين النطاقات دون كتلة `peligroso` صريحة
* **ProgramProfile** — يُنتج FormalGuard ملفاً شخصياً مختوماً ينتقل إلى CodeGen، ويربط التحليل الثابت بتوليد كود الآلة

## خوارزميات الكم كوظائف مدمجة

خوارزميات الكم هي بدائل أصلية للغة — يُصدر المترجم الدائرة ككود آلة:

* `grover(N)` — خوارزمية بحث Grover
* `qft(N)` — تحويل فورييه الكمي
* `shor(a, N)` — خوارزمية تحليل العوامل لـ Shor
* `deutsch_jozsa(N)` — خوارزمية Deutsch-Jozsa
* `bernstein_vazirani(N)` — خوارزمية Bernstein-Vazirani

بدون Qiskit. بدون تبعيات خارجية.

## تبعيات وقت التشغيل

**لا شيء.** الملف الثنائي الناتج عن Adsum لا يرتبط بمكتبات طرف ثالث:

* بدون libsodium
* بدون libcurl
* بدون libc
* بدون OpenSSL
* بدون Qiskit

المترجم نفسه مكتوب بـ Python ويتطلب Python 3.x للتشغيل.

## المعماريات المتاحة

| المنصة         | الحالة          |
|----------------|-----------------|
| x86_64 Linux       | متاح v1.9.0 |
| x86_64 Windows     | متاح v1.9.0 |
| ESP32-S3 معدن مجرد | متاح v1.9.0 |
| AArch64 Linux      | متاح v1.9.0 |

## الترخيص

وفقاً لاتفاقية ترخيص المستخدم النهائي (EULA) — ADSUM 1.x

---

# Adsum প্রোগ্রামিং ভাষা

মানুষের জন্য প্রোগ্রামিং ভাষা। জন্ম থেকেই নতুন প্যারাডাইমের জন্য ডিজাইন করা হয়েছে: ব্লকচেইন, IoT, রোবোটিক্স এবং নিউরাল সিস্টেম।

## কেন Adsum?

বর্তমান ভাষাগুলি বিংশ শতাব্দীর প্যারাডাইমের জন্য ডিজাইন করা হয়েছিল, কিন্তু বিশ্ব পরিবর্তিত হয়েছে এবং আজ আমরা বিকেন্দ্রীভূত সিস্টেম, এমবেডেড ডিভাইস, নিউরাল আর্কিটেকচার এবং হেটেরোজেনাস হার্ডওয়্যার তৈরি করি। Adsum এই নতুন প্যারাডাইমের জন্য জন্মেছে — অভিযোজন হিসেবে নয়, ভিত্তি হিসেবে।

## দর্শন

* **সোর্স কোডে সত্যিকারের বহুভাষী।** ভাষা বোধগম্য, মানুষের কাছাকাছি এবং আপনার সংস্কৃতি, অবস্থান বা দেশ নির্বিশেষে অ্যাক্সেসযোগ্য হওয়া উচিত।
* **শক্তিশালী টাইপিং এবং শব্দার্থিকভাবে নির্ধারক।**
* **সরাসরি মেশিন কোড জেনারেশন।** সম্পদ অপচয় করে এমন কোনো বিমূর্ত স্তর নেই।
* **ক্রিপ্টোগ্রাফি, ব্লকচেইন, নেটওয়ার্ক এবং AI-এর জন্য নেটিভ প্রিমিটিভ।** মূলের মৌলিক অংশ হিসেবে, লাইব্রেরি হিসেবে নয়।

## প্রকল্পের অবস্থা

বর্তমান সংস্করণ: **1.9.0**

## কম্পাইলার আর্কিটেকচার

Adsum একটি সম্পূর্ণ এন্ড-টু-এন্ড কম্পাইলার: যেকোনো মানব ভাষায় সোর্স কোড → চারটি আর্কিটেকচারে স্ট্যাটিক এক্সিকিউটেবল বাইনারি। LLVM, GCC, পার্সার জেনারেটর, বা রানটাইমে তৃতীয় পক্ষের লাইব্রেরি ছাড়াই।

১০-ধাপ পাইপলাইন:

```
সোর্স কোড (.adsum, যেকোনো মানব ভাষা)
  ↓
ধাপ 0  — বহুভাষী নর্মালাইজেশন          
  ↓
ধাপ 1  — প্রিপ্রোসেসিং (@resource)     
  ↓
ধাপ 2  — Lexer + Parser → AST          
  ↓
ধাপ 3  — আনুষ্ঠানিক চুক্তি লোড করা    
  ↓
ধাপ 4  — টাইপ চেকিং                   
  ↓
ধাপ 5  — ইন্টিগ্রিটি গার্ড            
  ↓
ধাপ 6  — ফর্মাল গার্ড                 
             ├── সেপারেশন লজিক        
             └── রিজিয়ন সলভার         
  ↓
ধাপ 7  — ফ্রেম বিল্ডার + কোডজেন ISA  
  ↓
ধাপ 8  — অ্যাসেম্বলিং                 
  ↓
ধাপ 9  — লিংকিং                       
  ↓
স্ট্যাটিক এক্সিকিউটেবল বাইনারি
```

## কম্পাইলেশন টার্গেট

একই সোর্স কোড প্রোগ্রাম পরিবর্তন না করেই যেকোনো টার্গেটে কম্পাইল হয়:

| ফরম্যাট  | প্ল্যাটফর্ম          |
|----------|----------------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 বেয়ার মেটাল |
| ELF64 AArch64 | AArch64 Linux  |

## সত্যিকারের বহুভাষী

Adsum ১৫টি প্রোগ্রামিং ভাষা সমর্থন করে:

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## ভাষাগত সার্বভৌমত্ব

Adsum ডকুমেন্টেশন অনুবাদ করে না, ভাষা নিজেই অনুবাদ করে। শব্দার্থ বা অভ্যন্তরীণ উপস্থাপনা পরিবর্তন না করেই কোড প্রোগ্রামারের মাতৃভাষায় লেখা যায়। নিরপেক্ষ IR একমাত্র আনুষ্ঠানিক চুক্তি হিসেবে কাজ করে, নিশ্চিত করে যে সমস্ত ভাষাগত রূপ একই বাইনারি ফলাফলে কম্পাইল হয়, যা সক্ষম করে:

* সফটওয়্যার উন্নয়নে সাংস্কৃতিক স্বাধীনতা
* ইংরেজির উপর নির্ভরতা ছাড়াই প্রযুক্তিগত অ্যাক্সেস
* ভাষাগুলির মধ্যে শব্দার্থিক অভিন্নতা
* নতুন মানব ভাষার দিকে স্কেলেবিলিটি

## আনুষ্ঠানিক চুক্তি (IR)

`system_manifest.json` কম্পাইলারের চুক্তিভিত্তিক মধ্যবর্তী উপস্থাপনা হিসেবে কাজ করে। এটি একমাত্র সত্যের উৎস যা একসাথে প্যারামিটার করে:

* **TypeChecker** — সমস্ত বিল্টইনের টাইপ সিগনেচার
* **SeparationLogicVerifier** — মেমোরি ডোমেন এবং ক্রস-ডোমেন নিয়ম (CDRs)
* **CodeGen** — ABI রেজিস্টার এবং আর্কিটেকচার অনুযায়ী `inject_*` ফাংশন

`ManifestLoader` এই চুক্তি লোড করে, ৬টি কঠোর ইনভেরিয়েন্ট যাচাই করে এবং তিনটি ভোক্তার কাছে বিতরণ করে। কোনো মডিউল সিগনেচার, ডোমেন বা রেজিস্টার হার্ডকোড করে না — সবকিছু manifest থেকে উদ্ভূত হয়।

## ক্রিপ্টোগ্রাফিক সার্বভৌমত্ব

Adsum-এর ক্রিপ্টোগ্রাফিক স্ট্যাক libsodium এবং OpenSSL-কে সম্পূর্ণরূপে প্রতিস্থাপন করে, প্রথম নীতি থেকে বাস্তবায়ন করে:

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** সাইন, ভেরিফাই, কীজেন (RFC 8032)
* **ChaCha20** এনক্রিপ্ট/ডিক্রিপ্ট (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** ধ্রুবক সময় সীমিত ক্ষেত্র গণিত
* **Ed25519 কার্ভ** ব্রাঞ্চলেস Montgomery ladder (CMOV) সহ

সমস্ত ক্রিপ্টোগ্রাফিক অপারেশন নেটিভ মেশিন কোড হিসেবে নির্গত হয়। spec/emit আর্কিটেকচার অ্যালগরিদমিক স্পেসিফিকেশনকে নির্গমন ব্যাকএন্ড থেকে আলাদা করে, মাল্টি-আর্কিটেকচার সমর্থন সক্ষম করে।

## নেটিভ নেটওয়ার্কিং

অপারেটিং সিস্টেম syscall দিয়ে সরাসরি TCP/IP এবং HTTP নেটওয়ার্ক বাস্তবায়িত:

* **DNS** — নেটিভ রেজোলিউশন (UDP পোর্ট 53), `/etc/resolv.conf` পড়ে
* **TCP** — libc ছাড়া connect, send, recv, close
* **HTTP** — হেডার বিল্ডার সহ GET/POST
* **Traceroute** — IP রুট ট্রেসিং (ICMP TTL)
* **ERFR** — নোড কনটেক্সট সহ নিজস্ব নেটওয়ার্ক প্রোটোকল

## IaG — প্রকৃত কৃত্রিম বুদ্ধিমত্তা

জৈবিকভাবে অনুপ্রাণিত নিউরাল মডেল যা AND, POPCNT, SHR এবং CMP নির্দেশে কম্পাইল হয়। কোনো ফ্লোটিং পয়েন্ট নেই, কোনো ম্যাট্রিক্স নেই, কোনো ব্যাকপ্রোপাগেশন নেই।

* **নিউরন** — ২৮-বিট অ্যাক্সন, ৭-বিট গেট সহ ৪টি শোয়ান কোষ
* **প্রক্রিয়াকরণ** — শোয়ান ফিল্টার করে (AND) এবং ফ্রিকোয়েন্সিতে রূপান্তরিত করে (POPCNT), ডেনড্রাইট থ্রেশহোল্ডের বিপরীতে তুলনা করে
* **টোপোলজি** — উদীয়মান পর্যায় সহ উল্টানো পাস্কাল ত্রিভুজ
* **প্রবৃত্তি** — JSON-এ প্রি-ওয়্যার্ড আচরণ (প্রতিরক্ষামূলক, মূল্যায়নমূলক, সক্রিয়, সামাজিক)
* **স্মৃতি** — সংবেদনশীল (১ টিক), স্বল্পমেয়াদী (১৬ স্ন্যাপশট), দীর্ঘমেয়াদী (ক্ষয়সহ ২৫৬ প্যাটার্ন)
* **নেটিভ প্রিমিটিভ** — axon_cargar, dendrita_disparar, udp_crear, json_a_texto ভাষার বিল্টইন হিসেবে উপলব্ধ

## আনুষ্ঠানিক যাচাইকরণ

গার্বেজ কালেক্টর ছাড়াই মেমোরি নিরাপত্তা নিশ্চিত করার গাণিতিক বাধা:

* **সেপারেশন লজিক** — প্রমাণ করে যে মেমোরি ডোমেনগুলি (SAFE_STACK, IO_VOLATILE, IA_SENSORY, CRYPTO_SECURE, IOT_REALTIME, QUANTUM) বিচ্ছিন্ন
* **রিজিয়ন সলভার** — স্ট্যাটিক এস্কেপ বিশ্লেষণ: লেক্সিকাল কনটেইনমেন্ট, ড্যাংলিং পয়েন্টার সনাক্তকরণ, স্থানীয় রেফারেন্সের এস্কেপ প্রতিরোধ
* **ইন্টিগ্রিটি গার্ড** — নাল ইন্টিগ্রিটি, স্পষ্ট মিউটেবিলিটি, `peligroso` ব্লক নিয়ন্ত্রণ
* **ক্রস-ডোমেন নিয়ম** — CDRs যা স্পষ্ট `peligroso` ব্লক ছাড়াই ডোমেনগুলির মধ্যে দূষণ রোধ করে
* **ProgramProfile** — FormalGuard একটি সিলড প্রোফাইল তৈরি করে যা CodeGen-এ যায়, স্ট্যাটিক বিশ্লেষণকে মেশিন কোড জেনারেশনের সাথে সংযুক্ত করে

## বিল্টইন হিসেবে কোয়ান্টাম অ্যালগরিদম

কোয়ান্টাম অ্যালগরিদম ভাষার নেটিভ প্রিমিটিভ — কম্পাইলার সার্কিটকে মেশিন কোড হিসেবে নির্গত করে:

* `grover(N)` — Grover অনুসন্ধান অ্যালগরিদম
* `qft(N)` — কোয়ান্টাম ফুরিয়ার ট্রান্সফর্ম
* `shor(a, N)` — Shor ফ্যাক্টরাইজেশন অ্যালগরিদম
* `deutsch_jozsa(N)` — Deutsch-Jozsa অ্যালগরিদম
* `bernstein_vazirani(N)` — Bernstein-Vazirani অ্যালগরিদম

কোনো Qiskit নেই। কোনো বাহ্যিক নির্ভরতা নেই।

## রানটাইম নির্ভরতা

**কোনোটি নেই।** ফলস্বরূপ Adsum বাইনারি কোনো তৃতীয় পক্ষের লাইব্রেরির সাথে লিঙ্ক করে না:

* কোনো libsodium নেই
* কোনো libcurl নেই
* কোনো libc নেই
* কোনো OpenSSL নেই
* কোনো Qiskit নেই

কম্পাইলার নিজেই Python-এ লেখা এবং চালাতে Python 3.x প্রয়োজন।

## উপলব্ধ আর্কিটেকচার

| প্ল্যাটফর্ম         | অবস্থা          |
|---------------------|-----------------|
| x86_64 Linux       | উপলব্ধ v1.9.0 |
| x86_64 Windows     | উপলব্ধ v1.9.0 |
| ESP32-S3 বেয়ার মেটাল | উপলব্ধ v1.9.0 |
| AArch64 Linux      | উপলব্ধ v1.9.0 |

## লাইসেন্স

চূড়ান্ত ব্যবহারকারী লাইসেন্স চুক্তি (EULA) অনুযায়ী — ADSUM 1.x

---

# Linguagem Adsum

A linguagem de programação para humanos. Desenhada desde o nascimento para os novos paradigmas: Blockchain, IoT, Robótica e Sistemas Neurais.

## Por que Adsum?

As linguagens atuais foram desenhadas para paradigmas do século XX, mas o mundo mudou e hoje desenvolvemos sistemas descentralizados, dispositivos embarcados, arquiteturas neurais e hardware heterogêneo. Adsum nasce para esses novos paradigmas — não como adaptação, mas como fundamento.

## Filosofia

* **Multilíngue real no código-fonte.** A linguagem deve ser compreensível, próxima ao humano e acessível independentemente da sua cultura, localização ou país.
* **Tipagem forte e semanticamente determinista.**
* **Geração direta de código de máquina.** Sem camadas de abstração que desperdicem recursos.
* **Primitivas nativas para criptografia, blockchain, rede e IA.** Como parte fundamental do núcleo, não como bibliotecas.

## Estado do Projeto

Versão atual: **1.9.0**

## Arquitetura do Compilador

Adsum é um compilador completo de ponta a ponta: código-fonte em qualquer idioma humano → binário estático executável em quatro arquiteturas. Sem LLVM, GCC, geradores de parsers, nem bibliotecas de terceiros em runtime.

Pipeline de 10 fases:

```
Código-fonte (.adsum, qualquer idioma humano)
  ↓
FASE 0  — Normalização multilíngue          
  ↓
FASE 1  — Pré-processamento (@resource)     
  ↓
FASE 2  — Lexer + Parser → AST             
  ↓
FASE 3  — Carregamento do Contrato Formal   
  ↓
FASE 4  — Verificação de Tipos             
  ↓
FASE 5  — Integrity Guard                  
  ↓
FASE 6  — Formal Guard                     
             ├── Separation Logic          
             └── Region Solver             
  ↓
FASE 7  — Frame Builder + Codegen ISA      
  ↓
FASE 8  — Montagem                         
  ↓
FASE 9  — Ligação                          
  ↓
Binário estático executável
```

## Alvos de Compilação

O mesmo código-fonte compila para qualquer alvo sem modificar o programa:

| Formato  | Plataforma          |
|----------|---------------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 bare metal |
| ELF64 AArch64 | AArch64 Linux  |

## Multilíngue Real

Adsum suporta 15 idiomas de programação:

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## Soberania Linguística

Adsum não traduz documentação, traduz a própria linguagem. O código pode ser escrito na língua materna do programador sem alterar a semântica nem a representação interna. O IR neutro atua como contrato formal único, garantindo que todas as variantes linguísticas compilem ao mesmo resultado binário, o que permite:

* Independência cultural no desenvolvimento de software
* Acesso técnico sem dependência do inglês
* Uniformidade semântica entre idiomas
* Escalabilidade para novos idiomas humanos

## Contrato Formal (IR)

O `system_manifest.json` atua como representação intermediária contratual do compilador. É a fonte única de verdade que parametriza simultaneamente:

* **TypeChecker** — assinaturas de tipos de todos os builtins
* **SeparationLogicVerifier** — domínios de memória e regras cross-domain (CDRs)
* **CodeGen** — registros ABI e funções `inject_*` por arquitetura

O `ManifestLoader` carrega este contrato, valida 6 invariantes estritas, e o distribui aos três consumidores. Nenhum módulo hardcodeia assinaturas, domínios nem registros — tudo se deriva do manifest.

## Soberania Criptográfica

O stack criptográfico de Adsum substitui completamente libsodium e OpenSSL, implementando desde primeiros princípios:

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** sign, verify, keygen (RFC 8032)
* **ChaCha20** encrypt/decrypt (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** aritmética de campo finito em tempo constante
* **Curva Ed25519** com Montgomery ladder branchless (CMOV)

Todas as operações criptográficas são emitidas como código de máquina nativo. A arquitetura spec/emit separa a especificação algorítmica do backend de emissão, habilitando suporte multi-arquitetura.

## Networking Nativo

Rede TCP/IP e HTTP implementadas diretamente com syscalls do sistema operacional:

* **DNS** — Resolução nativa (UDP porta 53), lê `/etc/resolv.conf`
* **TCP** — connect, send, recv, close sem libc
* **HTTP** — GET/POST com construtor de headers
* **Traceroute** — Rastreamento de rota IP (ICMP TTL)
* **ERFR** — Protocolo de rede próprio com contexto de nó

## IaG — Inteligência Artificial Genuína

Modelo neural biologicamente inspirado que compila para instruções AND, POPCNT, SHR e CMP. Sem ponto flutuante, sem matrizes, sem backpropagation.

* **Neurônio** — 28 bits de axônios, 4 células de Schwann com portas de 7 bits
* **Processamento** — Schwann filtra (AND) e converte em frequência (POPCNT), dendrito compara contra limiar
* **Topologia** — Triângulo de Pascal invertido com fases emergentes
* **Instintos** — Comportamentos pré-cabeados em JSON (defensivos, avaliativos, proativos, sociais)
* **Memória** — Sensorial (1 tick), curto prazo (16 snapshots), longo prazo (256 padrões com decaimento)
* **Primitivas nativas** — axon_cargar, dendrita_disparar, udp_criar, json_a_texto disponíveis como builtins da linguagem

## Verificação Formal

Barreira matemática que garante segurança de memória sem garbage collector:

* **Separation Logic** — Demonstra que os domínios de memória (SAFE_STACK, IO_VOLATILE, IA_SENSORY, CRYPTO_SECURE, IOT_REALTIME, QUANTUM) são disjuntos
* **Region Solver** — Análise de escape estática: contenção léxica, detecção de ponteiros pendentes, prevenção de escape de referências locais
* **Integrity Guard** — Null integrity, mutabilidade explícita, controle de blocos `peligroso`
* **Cross-Domain Rules** — CDRs que previnem contaminação entre domínios sem bloco `peligroso` explícito
* **ProgramProfile** — O FormalGuard produz um perfil selado que viaja ao CodeGen, conectando análise estática com geração de código de máquina

## Algoritmos Quânticos como Builtins

Os algoritmos quânticos são primitivas nativas da linguagem — o compilador emite o circuito como código de máquina:

* `grover(N)` — Algoritmo de busca de Grover
* `qft(N)` — Transformada Quântica de Fourier
* `shor(a, N)` — Algoritmo de fatoração de Shor
* `deutsch_jozsa(N)` — Algoritmo Deutsch-Jozsa
* `bernstein_vazirani(N)` — Algoritmo Bernstein-Vazirani

Sem Qiskit. Sem dependências externas.

## Dependências em Runtime

**Nenhuma.** O binário Adsum resultante não se vincula a bibliotecas de terceiros:

* Sem libsodium
* Sem libcurl
* Sem libc
* Sem OpenSSL
* Sem Qiskit

O compilador em si é escrito em Python e requer Python 3.x para executar.

## Arquiteturas Disponíveis

| Plataforma         | Estado          |
|--------------------|-----------------|
| x86_64 Linux       | Disponível v1.9.0 |
| x86_64 Windows     | Disponível v1.9.0 |
| ESP32-S3 bare metal | Disponível v1.9.0 |
| AArch64 Linux      | Disponível v1.9.0 |

## Licença

CONFORME O ACORDO DE LICENÇA DE USUÁRIO FINAL (EULA) — ADSUM 1.x

---

# Язык программирования Adsum

Язык программирования для людей. Разработан с самого начала для новых парадигм: блокчейн, IoT, робототехника и нейронные системы.

## Почему Adsum?

Существующие языки были разработаны для парадигм XX века, но мир изменился, и сегодня мы разрабатываем децентрализованные системы, встраиваемые устройства, нейронные архитектуры и неоднородное оборудование. Adsum рождён для этих новых парадигм — не как адаптация, а как основа.

## Философия

* **Настоящая многоязычность в исходном коде.** Язык должен быть понятным, близким к человеку и доступным независимо от вашей культуры, местоположения или страны.
* **Строгая типизация и семантически детерминированная.**
* **Прямая генерация машинного кода.** Без уровней абстракции, которые расходуют ресурсы впустую.
* **Нативные примитивы для криптографии, блокчейна, сети и ИИ.** Как фундаментальная часть ядра, а не библиотеки.

## Состояние проекта

Текущая версия: **1.9.0**

## Архитектура компилятора

Adsum — это полноценный сквозной компилятор: исходный код на любом человеческом языке → статический исполняемый бинарный файл для четырёх архитектур. Без LLVM, GCC, генераторов парсеров и сторонних библиотек во время выполнения.

Конвейер из 10 фаз:

```
Исходный код (.adsum, любой человеческий язык)
  ↓
ФАЗА 0  — Многоязычная нормализация          
  ↓
ФАЗА 1  — Препроцессинг (@resource)          
  ↓
ФАЗА 2  — Лексер + Парсер → AST             
  ↓
ФАЗА 3  — Загрузка формального контракта     
  ↓
ФАЗА 4  — Проверка типов                     
  ↓
ФАЗА 5  — Страж целостности                  
  ↓
ФАЗА 6  — Формальный страж                   
             ├── Логика разделения           
             └── Решатель регионов           
  ↓
ФАЗА 7  — Построитель фреймов + Codegen ISA  
  ↓
ФАЗА 8  — Ассемблирование                    
  ↓
ФАЗА 9  — Компоновка                         
  ↓
Статический исполняемый бинарный файл
```

## Цели компиляции

Один и тот же исходный код компилируется для любой цели без изменения программы:

| Формат  | Платформа          |
|---------|--------------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 bare metal |
| ELF64 AArch64 | AArch64 Linux  |

## Настоящая многоязычность

Adsum поддерживает 15 языков программирования:

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## Языковой суверенитет

Adsum переводит не документацию, а сам язык. Код можно писать на родном языке программиста, не изменяя семантику или внутреннее представление. Нейтральный IR действует как единственный формальный контракт, гарантируя, что все языковые варианты компилируются в одинаковый бинарный результат, что обеспечивает:

* Культурную независимость при разработке программного обеспечения
* Технический доступ без зависимости от английского языка
* Семантическую однородность между языками
* Масштабируемость к новым человеческим языкам

## Формальный контракт (IR)

`system_manifest.json` выступает как контрактное промежуточное представление компилятора. Это единственный источник истины, который одновременно параметризует:

* **TypeChecker** — сигнатуры типов всех встроенных функций
* **SeparationLogicVerifier** — домены памяти и правила кросс-доменных операций (CDR)
* **CodeGen** — регистры ABI и функции `inject_*` для каждой архитектуры

`ManifestLoader` загружает этот контракт, проверяет 6 строгих инвариантов и распределяет его трём потребителям. Ни один модуль не хардкодирует сигнатуры, домены или регистры — всё выводится из манифеста.

## Криптографический суверенитет

Криптографический стек Adsum полностью заменяет libsodium и OpenSSL, реализуя с первых принципов:

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** подпись, верификация, генерация ключей (RFC 8032)
* **ChaCha20** шифрование/дешифрование (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** арифметика конечного поля за постоянное время
* **Кривая Ed25519** с беспереходной лестницей Монтгомери (CMOV)

Все криптографические операции генерируются как нативный машинный код. Архитектура spec/emit отделяет алгоритмическую спецификацию от бэкенда генерации, обеспечивая поддержку нескольких архитектур.

## Нативная сеть

Сеть TCP/IP и HTTP реализованы напрямую через системные вызовы ОС:

* **DNS** — Нативное разрешение (UDP порт 53), читает `/etc/resolv.conf`
* **TCP** — connect, send, recv, close без libc
* **HTTP** — GET/POST с построителем заголовков
* **Traceroute** — Трассировка маршрута IP (ICMP TTL)
* **ERFR** — Собственный сетевой протокол с контекстом узла

## IaG — Подлинный искусственный интеллект

Биологически вдохновлённая нейронная модель, компилируемая в инструкции AND, POPCNT, SHR и CMP. Без чисел с плавающей точкой, без матриц, без обратного распространения.

* **Нейрон** — 28 бит аксонов, 4 клетки Шванна с вентилями 7 бит
* **Обработка** — Шванн фильтрует (AND) и конвертирует в частоту (POPCNT), дендрит сравнивает с порогом
* **Топология** — Перевёрнутый треугольник Паскаля с эмерджентными фазами
* **Инстинкты** — Предзаписанное поведение в JSON (защитное, оценочное, проактивное, социальное)
* **Память** — Сенсорная (1 такт), краткосрочная (16 снимков), долгосрочная (256 паттернов с затуханием)
* **Нативные примитивы** — axon_cargar, dendrita_disparar, udp_crear, json_a_texto доступны как встроенные функции языка

## Формальная верификация

Математический барьер, гарантирующий безопасность памяти без сборщика мусора:

* **Логика разделения** — Доказывает, что домены памяти (SAFE_STACK, IO_VOLATILE, IA_SENSORY, CRYPTO_SECURE, IOT_REALTIME, QUANTUM) не пересекаются
* **Решатель регионов** — Статический анализ утечек: лексическое сдерживание, обнаружение висячих указателей, предотвращение утечек локальных ссылок
* **Страж целостности** — Null integrity, явная изменяемость, контроль блоков `peligroso`
* **Кросс-доменные правила** — CDR, предотвращающие загрязнение между доменами без явного блока `peligroso`
* **ProgramProfile** — FormalGuard создаёт запечатанный профиль, который передаётся в CodeGen, связывая статический анализ с генерацией машинного кода

## Квантовые алгоритмы как встроенные функции

Квантовые алгоритмы являются нативными примитивами языка — компилятор генерирует схему как машинный код:

* `grover(N)` — Алгоритм поиска Гровера
* `qft(N)` — Квантовое преобразование Фурье
* `shor(a, N)` — Алгоритм факторизации Шора
* `deutsch_jozsa(N)` — Алгоритм Дойча-Йожи
* `bernstein_vazirani(N)` — Алгоритм Бернштейна-Вазирани

Без Qiskit. Без внешних зависимостей.

## Зависимости во время выполнения

**Никаких.** Результирующий бинарный файл Adsum не компонуется ни с одной сторонней библиотекой:

* Без libsodium
* Без libcurl
* Без libc
* Без OpenSSL
* Без Qiskit

Сам компилятор написан на Python и требует Python 3.x для запуска.

## Доступные архитектуры

| Платформа         | Статус          |
|-------------------|-----------------|
| x86_64 Linux       | Доступно v1.9.0 |
| x86_64 Windows     | Доступно v1.9.0 |
| ESP32-S3 bare metal | Доступно v1.9.0 |
| AArch64 Linux      | Доступно v1.9.0 |

## Лицензия

СОГЛАСНО ЛИЦЕНЗИОННОМУ СОГЛАШЕНИЮ С КОНЕЧНЫМ ПОЛЬЗОВАТЕЛЕМ (EULA) — ADSUM 1.x

---

# Adsumプログラミング言語

人間のためのプログラミング言語。ブロックチェーン、IoT、ロボティクス、ニューラルシステムという新しいパラダイムのために生まれた瞬間から設計されています。

## なぜAdsum？

現在の言語は20世紀のパラダイムのために設計されましたが、世界は変わりました。今日、私たちは分散型システム、組み込みデバイス、ニューラルアーキテクチャ、異種ハードウェアを開発しています。Adsumはこれらの新しいパラダイムのために生まれました — 適応としてではなく、基盤として。

## 哲学

* **ソースコードにおける真の多言語。** 言語は、文化、場所、国に関係なく、理解しやすく、人間に近く、アクセスしやすいものでなければなりません。
* **強い型付けと意味論的に決定論的。**
* **マシンコードの直接生成。** リソースを無駄にする抽象化レイヤーなし。
* **暗号、ブロックチェーン、ネットワーク、AIのためのネイティブプリミティブ。** ライブラリとしてではなく、コアの根本的な部分として。

## プロジェクト状態

現在のバージョン: **1.9.0**

## コンパイラアーキテクチャ

Adsumは完全なエンドツーエンドコンパイラです：任意の人間言語のソースコード → 四つのアーキテクチャの静的実行可能バイナリ。LLVM、GCC、パーサージェネレーター、ランタイムのサードパーティライブラリなし。

10フェーズパイプライン：

```
ソースコード (.adsum, 任意の人間言語)
  ↓
フェーズ 0  — 多言語正規化          
  ↓
フェーズ 1  — 前処理 (@resource)   
  ↓
フェーズ 2  — レクサー + パーサー → AST  
  ↓
フェーズ 3  — 正式契約の読み込み    
  ↓
フェーズ 4  — 型チェック            
  ↓
フェーズ 5  — インテグリティガード  
  ↓
フェーズ 6  — フォーマルガード      
             ├── 分離論理          
             └── リージョンソルバー 
  ↓
フェーズ 7  — フレームビルダー + コードジェン ISA  
  ↓
フェーズ 8  — アセンブリ            
  ↓
フェーズ 9  — リンク                
  ↓
静的実行可能バイナリ
```

## コンパイルターゲット

同じソースコードがプログラムを変更せずに任意のターゲットにコンパイルされます：

| フォーマット  | プラットフォーム          |
|--------------|---------------------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 ベアメタル |
| ELF64 AArch64 | AArch64 Linux  |

## 真の多言語

Adsumは15のプログラミング言語をサポートします：

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## 言語的主権

Adsumはドキュメントを翻訳するのではなく、言語そのものを翻訳します。コードは意味論や内部表現を変えることなく、プログラマーの母国語で書くことができます。中立的なIRは唯一の正式契約として機能し、すべての言語バリアントが同じバイナリ結果にコンパイルされることを保証します：

* ソフトウェア開発における文化的独立性
* 英語への依存なしの技術的アクセス
* 言語間の意味論的均一性
* 新しい人間言語へのスケーラビリティ

## 正式契約 (IR)

`system_manifest.json`はコンパイラの契約的中間表現として機能します。同時にパラメータ化する唯一の真実の源です：

* **TypeChecker** — すべての組み込みの型シグネチャ
* **SeparationLogicVerifier** — メモリドメインとクロスドメインルール (CDR)
* **CodeGen** — ABIレジスタとアーキテクチャ別の `inject_*` 関数

`ManifestLoader`はこの契約をロードし、6つの厳格な不変条件を検証し、3つのコンシューマーに配布します。どのモジュールもシグネチャ、ドメイン、レジスタをハードコードしません — すべてはマニフェストから派生します。

## 暗号的主権

Adsumの暗号スタックはlibsodiumとOpenSSLを完全に置き換え、第一原理から実装します：

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** 署名、検証、鍵生成 (RFC 8032)
* **ChaCha20** 暗号化/復号化 (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** 定数時間有限体演算
* **Ed25519曲線** ブランチレスMontgomeryラダー (CMOV) 付き

すべての暗号操作はネイティブマシンコードとして出力されます。spec/emit アーキテクチャはアルゴリズム仕様を出力バックエンドから分離し、マルチアーキテクチャサポートを可能にします。

## ネイティブネットワーキング

TCP/IPおよびHTTPネットワークはOSのシスコールで直接実装：

* **DNS** — ネイティブ解決 (UDPポート53)、`/etc/resolv.conf` を読む
* **TCP** — libc なしの connect、send、recv、close
* **HTTP** — ヘッダービルダー付きGET/POST
* **Traceroute** — IPルートトレース (ICMP TTL)
* **ERFR** — ノードコンテキスト付き独自ネットワークプロトコル

## IaG — 真の人工知能

AND、POPCNT、SHR、CMP命令にコンパイルする生物学的にインスパイアされたニューラルモデル。浮動小数点なし、行列なし、バックプロパゲーションなし。

* **ニューロン** — 28ビットの軸索、7ビットゲート付き4つのシュワン細胞
* **処理** — シュワンがフィルタリング (AND) して周波数に変換 (POPCNT)、樹状突起が閾値と比較
* **トポロジー** — 創発フェーズ付き逆パスカル三角形
* **本能** — JSONに事前配線された動作 (防御的、評価的、積極的、社会的)
* **メモリ** — 感覚 (1ティック)、短期 (16スナップショット)、長期 (減衰付き256パターン)
* **ネイティブプリミティブ** — axon_cargar、dendrita_disparar、udp_crear、json_a_textoが言語の組み込みとして利用可能

## 形式検証

ガベージコレクターなしでメモリ安全性を保証する数学的バリア：

* **分離論理** — メモリドメイン (SAFE_STACK、IO_VOLATILE、IA_SENSORY、CRYPTO_SECURE、IOT_REALTIME、QUANTUM) が互いに素であることを証明
* **リージョンソルバー** — 静的エスケープ解析：字句的封じ込め、ダングリングポインター検出、ローカル参照のエスケープ防止
* **インテグリティガード** — Nullインテグリティ、明示的な可変性、`peligroso` ブロック制御
* **クロスドメインルール** — 明示的な `peligroso` ブロックなしのドメイン間汚染を防ぐCDR
* **ProgramProfile** — FormalGuardが封印されたプロファイルを生成してCodeGenに渡し、静的解析とマシンコード生成を接続

## 組み込みとしての量子アルゴリズム

量子アルゴリズムは言語のネイティブプリミティブです — コンパイラは回路をマシンコードとして出力します：

* `grover(N)` — Groverの探索アルゴリズム
* `qft(N)` — 量子フーリエ変換
* `shor(a, N)` — Shorの素因数分解アルゴリズム
* `deutsch_jozsa(N)` — Deutsch-Jozsaアルゴリズム
* `bernstein_vazirani(N)` — Bernstein-Vaziraniアルゴリズム

Qiskit なし。外部依存なし。

## ランタイム依存関係

**なし。** 生成されたAdsumバイナリはサードパーティライブラリとリンクしません：

* libsodium なし
* libcurl なし
* libc なし
* OpenSSL なし
* Qiskit なし

コンパイラ自体はPythonで書かれており、実行にはPython 3.xが必要です。

## 利用可能なアーキテクチャ

| プラットフォーム         | 状態          |
|--------------------------|---------------|
| x86_64 Linux       | 利用可能 v1.9.0 |
| x86_64 Windows     | 利用可能 v1.9.0 |
| ESP32-S3 ベアメタル | 利用可能 v1.9.0 |
| AArch64 Linux      | 利用可能 v1.9.0 |

## ライセンス

エンドユーザーライセンス契約 (EULA) に従って — ADSUM 1.x

---

# Bahasa Pemrograman Adsum

Bahasa pemrograman untuk manusia. Dirancang sejak lahir untuk paradigma baru: Blockchain, IoT, Robotika, dan Sistem Neural.

## Mengapa Adsum?

Bahasa-bahasa saat ini dirancang untuk paradigma abad ke-20, tetapi dunia telah berubah dan sekarang kita mengembangkan sistem terdesentralisasi, perangkat tertanam, arsitektur neural, dan perangkat keras heterogen. Adsum lahir untuk paradigma baru tersebut — bukan sebagai adaptasi, melainkan sebagai fondasi.

## Filosofi

* **Multibahasa nyata dalam kode sumber.** Bahasa harus mudah dipahami, dekat dengan manusia, dan dapat diakses tanpa memandang budaya, lokasi, atau negara Anda.
* **Pengetikan kuat dan deterministik secara semantik.**
* **Generasi kode mesin langsung.** Tanpa lapisan abstraksi yang memboroskan sumber daya.
* **Primitif asli untuk kriptografi, blockchain, jaringan, dan AI.** Sebagai bagian fundamental dari inti, bukan sebagai pustaka.

## Status Proyek

Versi saat ini: **1.9.0**

## Arsitektur Kompiler

Adsum adalah kompiler lengkap dari ujung ke ujung: kode sumber dalam bahasa manusia apa pun → biner statis yang dapat dieksekusi dalam empat arsitektur. Tanpa LLVM, GCC, generator parser, maupun pustaka pihak ketiga dalam runtime.

Pipeline 10 fase:

```
Kode sumber (.adsum, bahasa manusia apa pun)
  ↓
FASE 0  — Normalisasi multibahasa          
  ↓
FASE 1  — Prapemrosesan (@resource)        
  ↓
FASE 2  — Lexer + Parser → AST            
  ↓
FASE 3  — Memuat Kontrak Formal            
  ↓
FASE 4  — Pemeriksaan Tipe                
  ↓
FASE 5  — Integrity Guard                  
  ↓
FASE 6  — Formal Guard                     
             ├── Separation Logic          
             └── Region Solver             
  ↓
FASE 7  — Frame Builder + Codegen ISA     
  ↓
FASE 8  — Perakitan                        
  ↓
FASE 9  — Penautan                         
  ↓
Biner statis yang dapat dieksekusi
```

## Target Kompilasi

Kode sumber yang sama dikompilasi ke target mana pun tanpa mengubah program:

| Format  | Platform          |
|---------|-------------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 bare metal |
| ELF64 AArch64 | AArch64 Linux  |

## Multibahasa Nyata

Adsum mendukung 15 bahasa pemrograman:

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## Kedaulatan Linguistik

Adsum tidak menerjemahkan dokumentasi, melainkan menerjemahkan bahasa itu sendiri. Kode dapat ditulis dalam bahasa ibu pemrogram tanpa mengubah semantik maupun representasi internalnya. IR netral bertindak sebagai kontrak formal tunggal, menjamin bahwa semua varian linguistik dikompilasi ke hasil biner yang sama, yang memungkinkan:

* Kemandirian budaya dalam pengembangan perangkat lunak
* Akses teknis tanpa ketergantungan pada bahasa Inggris
* Keseragaman semantik antarbahasa
* Skalabilitas menuju bahasa manusia baru

## Kontrak Formal (IR)

`system_manifest.json` bertindak sebagai representasi perantara kontraktual kompiler. Ini adalah satu-satunya sumber kebenaran yang secara bersamaan memparameterisasi:

* **TypeChecker** — tanda tangan tipe semua builtin
* **SeparationLogicVerifier** — domain memori dan aturan lintas domain (CDR)
* **CodeGen** — register ABI dan fungsi `inject_*` per arsitektur

`ManifestLoader` memuat kontrak ini, memvalidasi 6 invarian ketat, dan mendistribusikannya ke tiga konsumen. Tidak ada modul yang hardcode tanda tangan, domain, maupun register — semuanya berasal dari manifest.

## Kedaulatan Kriptografi

Stack kriptografi Adsum sepenuhnya menggantikan libsodium dan OpenSSL, mengimplementasikan dari prinsip pertama:

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** sign, verify, keygen (RFC 8032)
* **ChaCha20** encrypt/decrypt (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** aritmetika bidang terbatas waktu konstan
* **Kurva Ed25519** dengan Montgomery ladder branchless (CMOV)

Semua operasi kriptografi dipancarkan sebagai kode mesin asli. Arsitektur spec/emit memisahkan spesifikasi algoritmik dari backend emisi, memungkinkan dukungan multi-arsitektur.

## Jaringan Asli

Jaringan TCP/IP dan HTTP diimplementasikan langsung dengan syscall sistem operasi:

* **DNS** — Resolusi asli (UDP port 53), membaca `/etc/resolv.conf`
* **TCP** — connect, send, recv, close tanpa libc
* **HTTP** — GET/POST dengan pembangun header
* **Traceroute** — Pelacakan rute IP (ICMP TTL)
* **ERFR** — Protokol jaringan sendiri dengan konteks node

## IaG — Kecerdasan Buatan Sejati

Model neural yang terinspirasi secara biologis yang dikompilasi ke instruksi AND, POPCNT, SHR, dan CMP. Tanpa titik mengambang, tanpa matriks, tanpa backpropagation.

* **Neuron** — 28 bit akson, 4 sel Schwann dengan gerbang 7 bit
* **Pemrosesan** — Schwann menyaring (AND) dan mengonversi ke frekuensi (POPCNT), dendrit membandingkan terhadap ambang batas
* **Topologi** — Segitiga Pascal terbalik dengan fase emergen
* **Naluri** — Perilaku yang dikabelkan sebelumnya dalam JSON (defensif, evaluatif, proaktif, sosial)
* **Memori** — Sensoris (1 tick), jangka pendek (16 snapshot), jangka panjang (256 pola dengan peluruhan)
* **Primitif asli** — axon_cargar, dendrita_disparar, udp_crear, json_a_texto tersedia sebagai builtin bahasa

## Verifikasi Formal

Hambatan matematis yang menjamin keamanan memori tanpa pengumpul sampah:

* **Separation Logic** — Membuktikan bahwa domain memori (SAFE_STACK, IO_VOLATILE, IA_SENSORY, CRYPTO_SECURE, IOT_REALTIME, QUANTUM) saling lepas
* **Region Solver** — Analisis pelolosan statis: penahanan leksikal, deteksi pointer menggantung, pencegahan pelolosan referensi lokal
* **Integrity Guard** — Integritas null, mutabilitas eksplisit, kontrol blok `peligroso`
* **Cross-Domain Rules** — CDR yang mencegah kontaminasi antardomain tanpa blok `peligroso` eksplisit
* **ProgramProfile** — FormalGuard menghasilkan profil tersegel yang dikirim ke CodeGen, menghubungkan analisis statis dengan pembuatan kode mesin

## Algoritma Kuantum sebagai Builtin

Algoritma kuantum adalah primitif asli bahasa — kompiler memancarkan sirkuit sebagai kode mesin:

* `grover(N)` — Algoritma pencarian Grover
* `qft(N)` — Transformasi Fourier Kuantum
* `shor(a, N)` — Algoritma faktorisasi Shor
* `deutsch_jozsa(N)` — Algoritma Deutsch-Jozsa
* `bernstein_vazirani(N)` — Algoritma Bernstein-Vazirani

Tanpa Qiskit. Tanpa ketergantungan eksternal.

## Ketergantungan Runtime

**Tidak ada.** Biner Adsum yang dihasilkan tidak terhubung ke pustaka pihak ketiga mana pun:

* Tanpa libsodium
* Tanpa libcurl
* Tanpa libc
* Tanpa OpenSSL
* Tanpa Qiskit

Kompiler itu sendiri ditulis dalam Python dan memerlukan Python 3.x untuk berjalan.

## Arsitektur yang Tersedia

| Platform         | Status          |
|------------------|-----------------|
| x86_64 Linux       | Tersedia v1.9.0 |
| x86_64 Windows     | Tersedia v1.9.0 |
| ESP32-S3 bare metal | Tersedia v1.9.0 |
| AArch64 Linux      | Tersedia v1.9.0 |

## Lisensi

SESUAI PERJANJIAN LISENSI PENGGUNA AKHIR (EULA) — ADSUM 1.x

---

# Langage Adsum

Le langage de programmation pour les humains. Conçu dès sa naissance pour les nouveaux paradigmes : Blockchain, IoT, Robotique et Systèmes Neuronaux.

## Pourquoi Adsum ?

Les langages actuels ont été conçus pour les paradigmes du XXe siècle, mais le monde a changé et aujourd'hui nous développons des systèmes décentralisés, des dispositifs embarqués, des architectures neuronales et du matériel hétérogène. Adsum naît pour ces nouveaux paradigmes — non pas comme adaptation, mais comme fondement.

## Philosophie

* **Vrai multilinguisme dans le code source.** Le langage doit être compréhensible, proche de l'humain et accessible quelle que soit votre culture, votre localisation ou votre pays.
* **Typage fort et sémantiquement déterministe.**
* **Génération directe de code machine.** Sans couches d'abstraction qui gaspillent les ressources.
* **Primitives natives pour la cryptographie, la blockchain, le réseau et l'IA.** Comme partie fondamentale du noyau, pas comme bibliothèques.

## État du Projet

Version actuelle : **1.9.0**

## Architecture du Compilateur

Adsum est un compilateur complet de bout en bout : code source dans n'importe quelle langue humaine → binaire statique exécutable sur quatre architectures. Sans LLVM, GCC, générateurs de parsers, ni bibliothèques tierces en runtime.

Pipeline de 10 phases :

```
Code source (.adsum, n'importe quelle langue humaine)
  ↓
PHASE 0  — Normalisation multilingue          
  ↓
PHASE 1  — Prétraitement (@resource)          
  ↓
PHASE 2  — Lexer + Parser → AST              
  ↓
PHASE 3  — Chargement du Contrat Formel       
  ↓
PHASE 4  — Vérification de Types             
  ↓
PHASE 5  — Integrity Guard                    
  ↓
PHASE 6  — Formal Guard                       
             ├── Separation Logic             
             └── Region Solver                
  ↓
PHASE 7  — Frame Builder + Codegen ISA        
  ↓
PHASE 8  — Assemblage                         
  ↓
PHASE 9  — Liaison                            
  ↓
Binaire statique exécutable
```

## Cibles de Compilation

Le même code source se compile vers n'importe quelle cible sans modifier le programme :

| Format  | Plateforme          |
|---------|---------------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 bare metal |
| ELF64 AArch64 | AArch64 Linux  |

## Vrai Multilinguisme

Adsum supporte 15 langues de programmation :

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## Souveraineté Linguistique

Adsum ne traduit pas la documentation, il traduit le langage lui-même. Le code peut être écrit dans la langue maternelle du programmeur sans altérer la sémantique ni la représentation interne. L'IR neutre agit comme contrat formel unique, garantissant que toutes les variantes linguistiques compilent vers le même résultat binaire, ce qui permet :

* L'indépendance culturelle dans le développement logiciel
* L'accès technique sans dépendance à l'anglais
* L'uniformité sémantique entre les langues
* La scalabilité vers de nouvelles langues humaines

## Contrat Formel (IR)

Le `system_manifest.json` agit comme représentation intermédiaire contractuelle du compilateur. C'est la source unique de vérité qui paramétrise simultanément :

* **TypeChecker** — signatures de types de tous les builtins
* **SeparationLogicVerifier** — domaines mémoire et règles cross-domain (CDR)
* **CodeGen** — registres ABI et fonctions `inject_*` par architecture

Le `ManifestLoader` charge ce contrat, valide 6 invariants stricts, et le distribue aux trois consommateurs. Aucun module ne code en dur des signatures, domaines ou registres — tout est dérivé du manifest.

## Souveraineté Cryptographique

La pile cryptographique d'Adsum remplace complètement libsodium et OpenSSL, implémentant depuis les premiers principes :

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** sign, verify, keygen (RFC 8032)
* **ChaCha20** encrypt/decrypt (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** arithmétique de corps fini en temps constant
* **Courbe Ed25519** avec Montgomery ladder branchless (CMOV)

Toutes les opérations cryptographiques sont émises en code machine natif. L'architecture spec/emit sépare la spécification algorithmique du backend d'émission, permettant le support multi-architecture.

## Réseau Natif

Réseau TCP/IP et HTTP implémentés directement avec les syscalls du système d'exploitation :

* **DNS** — Résolution native (UDP port 53), lit `/etc/resolv.conf`
* **TCP** — connect, send, recv, close sans libc
* **HTTP** — GET/POST avec constructeur de headers
* **Traceroute** — Tracé de route IP (ICMP TTL)
* **ERFR** — Protocole réseau propre avec contexte de nœud

## IaG — Intelligence Artificielle Genuïne

Modèle neuronal biologiquement inspiré qui compile en instructions AND, POPCNT, SHR et CMP. Sans virgule flottante, sans matrices, sans rétropropagation.

* **Neurone** — 28 bits d'axones, 4 cellules de Schwann avec portes de 7 bits
* **Traitement** — Schwann filtre (AND) et convertit en fréquence (POPCNT), dendrite compare contre le seuil
* **Topologie** — Triangle de Pascal inversé avec phases émergentes
* **Instincts** — Comportements précâblés en JSON (défensifs, évaluatifs, proactifs, sociaux)
* **Mémoire** — Sensorielle (1 tick), court terme (16 snapshots), long terme (256 patterns avec décroissance)
* **Primitives natives** — axon_cargar, dendrita_disparar, udp_crear, json_a_texto disponibles comme builtins du langage

## Vérification Formelle

Barrière mathématique garantissant la sécurité mémoire sans ramasse-miettes :

* **Separation Logic** — Démontre que les domaines mémoire (SAFE_STACK, IO_VOLATILE, IA_SENSORY, CRYPTO_SECURE, IOT_REALTIME, QUANTUM) sont disjoints
* **Region Solver** — Analyse d'échappement statique : confinement lexical, détection de pointeurs pendants, prévention d'échappement de références locales
* **Integrity Guard** — Intégrité null, mutabilité explicite, contrôle des blocs `peligroso`
* **Cross-Domain Rules** — CDR qui préviennent la contamination entre domaines sans bloc `peligroso` explicite
* **ProgramProfile** — Le FormalGuard produit un profil scellé qui voyage au CodeGen, connectant l'analyse statique à la génération de code machine

## Algorithmes Quantiques comme Builtins

Les algorithmes quantiques sont des primitives natives du langage — le compilateur émet le circuit comme code machine :

* `grover(N)` — Algorithme de recherche de Grover
* `qft(N)` — Transformée de Fourier Quantique
* `shor(a, N)` — Algorithme de factorisation de Shor
* `deutsch_jozsa(N)` — Algorithme Deutsch-Jozsa
* `bernstein_vazirani(N)` — Algorithme Bernstein-Vazirani

Sans Qiskit. Sans dépendances externes.

## Dépendances en Runtime

**Aucune.** Le binaire Adsum résultant ne se lie à aucune bibliothèque tierce :

* Sans libsodium
* Sans libcurl
* Sans libc
* Sans OpenSSL
* Sans Qiskit

Le compilateur lui-même est écrit en Python et nécessite Python 3.x pour s'exécuter.

## Architectures Disponibles

| Plateforme         | État          |
|--------------------|---------------|
| x86_64 Linux       | Disponible v1.9.0 |
| x86_64 Windows     | Disponible v1.9.0 |
| ESP32-S3 bare metal | Disponible v1.9.0 |
| AArch64 Linux      | Disponible v1.9.0 |

## Licence

CONFORMÉMENT À L'ACCORD DE LICENCE UTILISATEUR FINAL (EULA) — ADSUM 1.x

---

# Adsum Programmierspache

Die Programmiersprache für Menschen. Von Geburt an für neue Paradigmen entworfen: Blockchain, IoT, Robotik und Neuronale Systeme.

## Warum Adsum?

Die aktuellen Sprachen wurden für Paradigmen des 20. Jahrhunderts entworfen, aber die Welt hat sich verändert und heute entwickeln wir dezentralisierte Systeme, eingebettete Geräte, neuronale Architekturen und heterogene Hardware. Adsum wurde für diese neuen Paradigmen geboren — nicht als Anpassung, sondern als Fundament.

## Philosophie

* **Echte Mehrsprachigkeit im Quellcode.** Die Sprache muss verständlich, menschennah und zugänglich sein, unabhängig von Kultur, Standort oder Land.
* **Starke Typisierung und semantisch deterministisch.**
* **Direkte Maschinencodegeenerierung.** Ohne Abstraktionsschichten, die Ressourcen verschwenden.
* **Native Primitive für Kryptografie, Blockchain, Netzwerk und KI.** Als fundamentaler Teil des Kerns, nicht als Bibliotheken.

## Projektstatus

Aktuelle Version: **1.9.0**

## Compiler-Architektur

Adsum ist ein vollständiger End-to-End-Compiler: Quellcode in jeder menschlichen Sprache → statisch ausführbare Binärdatei für vier Architekturen. Ohne LLVM, GCC, Parser-Generatoren oder Drittanbieter-Bibliotheken zur Laufzeit.

10-Phasen-Pipeline:

```
Quellcode (.adsum, jede menschliche Sprache)
  ↓
PHASE 0  — Mehrsprachige Normalisierung          
  ↓
PHASE 1  — Vorverarbeitung (@resource)           
  ↓
PHASE 2  — Lexer + Parser → AST                 
  ↓
PHASE 3  — Laden des Formalen Vertrags           
  ↓
PHASE 4  — Typüberprüfung                        
  ↓
PHASE 5  — Integrity Guard                        
  ↓
PHASE 6  — Formal Guard                           
             ├── Separation Logic                 
             └── Region Solver                    
  ↓
PHASE 7  — Frame Builder + Codegen ISA           
  ↓
PHASE 8  — Assemblierung                          
  ↓
PHASE 9  — Verknüpfung                            
  ↓
Statisch ausführbare Binärdatei
```

## Kompilierungsziele

Derselbe Quellcode kompiliert zu jedem Ziel, ohne das Programm zu ändern:

| Format  | Plattform          |
|---------|--------------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 bare metal |
| ELF64 AArch64 | AArch64 Linux  |

## Echte Mehrsprachigkeit

Adsum unterstützt 15 Programmiersprachen:

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## Sprachliche Souveränität

Adsum übersetzt keine Dokumentation, sondern die Sprache selbst. Code kann in der Muttersprache des Programmierers geschrieben werden, ohne die Semantik oder interne Darstellung zu ändern. Das neutrale IR fungiert als einziger formaler Vertrag und garantiert, dass alle sprachlichen Varianten zum gleichen binären Ergebnis kompilieren, was ermöglicht:

* Kulturelle Unabhängigkeit in der Softwareentwicklung
* Technischen Zugang ohne Abhängigkeit vom Englischen
* Semantische Einheitlichkeit zwischen Sprachen
* Skalierbarkeit zu neuen menschlichen Sprachen

## Formaler Vertrag (IR)

Die `system_manifest.json` fungiert als vertragliche Zwischendarstellung des Compilers. Es ist die einzige Wahrheitsquelle, die gleichzeitig parametrisiert:

* **TypeChecker** — Typsignaturen aller Builtins
* **SeparationLogicVerifier** — Speicherdomänen und Cross-Domain-Regeln (CDR)
* **CodeGen** — ABI-Register und `inject_*`-Funktionen pro Architektur

Der `ManifestLoader` lädt diesen Vertrag, validiert 6 strenge Invarianten und verteilt ihn an die drei Konsumenten. Kein Modul hardcodiert Signaturen, Domänen oder Register — alles wird aus dem Manifest abgeleitet.

## Kryptografische Souveränität

Adsums kryptografischer Stack ersetzt libsodium und OpenSSL vollständig und implementiert von ersten Prinzipien:

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** sign, verify, keygen (RFC 8032)
* **ChaCha20** encrypt/decrypt (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** Endkörperarithmetik in konstanter Zeit
* **Ed25519-Kurve** mit branchless Montgomery-Leiter (CMOV)

Alle kryptografischen Operationen werden als nativer Maschinencode ausgegeben. Die spec/emit-Architektur trennt die algorithmische Spezifikation vom Ausgabe-Backend und ermöglicht Multi-Architektur-Unterstützung.

## Natives Netzwerk

TCP/IP- und HTTP-Netzwerk direkt mit OS-Syscalls implementiert:

* **DNS** — Native Auflösung (UDP Port 53), liest `/etc/resolv.conf`
* **TCP** — connect, send, recv, close ohne libc
* **HTTP** — GET/POST mit Header-Builder
* **Traceroute** — IP-Routen-Verfolgung (ICMP TTL)
* **ERFR** — Eigenes Netzwerkprotokoll mit Knotenkontext

## IaG — Echte Künstliche Intelligenz

Biologisch inspiriertes neuronales Modell, das zu AND-, POPCNT-, SHR- und CMP-Anweisungen kompiliert. Ohne Gleitkomma, ohne Matrizen, ohne Backpropagation.

* **Neuron** — 28-Bit-Axone, 4 Schwann-Zellen mit 7-Bit-Gattern
* **Verarbeitung** — Schwann filtert (AND) und konvertiert zu Frequenz (POPCNT), Dendrit vergleicht mit Schwellenwert
* **Topologie** — Umgekehrtes Pascalsches Dreieck mit emergenten Phasen
* **Instinkte** — In JSON vorkonfigurierte Verhaltensweisen (defensiv, evaluativ, proaktiv, sozial)
* **Gedächtnis** — Sensorisch (1 Tick), Kurzzeit (16 Snapshots), Langzeit (256 Muster mit Zerfall)
* **Native Primitive** — axon_cargar, dendrita_disparar, udp_crear, json_a_texto als Sprach-Builtins verfügbar

## Formale Verifikation

Mathematische Barriere, die Speichersicherheit ohne Garbage Collector garantiert:

* **Separation Logic** — Beweist, dass Speicherdomänen (SAFE_STACK, IO_VOLATILE, IA_SENSORY, CRYPTO_SECURE, IOT_REALTIME, QUANTUM) disjunkt sind
* **Region Solver** — Statische Escape-Analyse: lexikalische Eindämmung, Erkennung hängender Zeiger, Verhinderung von Flucht lokaler Referenzen
* **Integrity Guard** — Null-Integrität, explizite Veränderlichkeit, Steuerung von `peligroso`-Blöcken
* **Cross-Domain-Regeln** — CDR, die die Kontamination zwischen Domänen ohne expliziten `peligroso`-Block verhindern
* **ProgramProfile** — FormalGuard erzeugt ein versiegeltes Profil, das zum CodeGen reist und statische Analyse mit Maschinecodegeenerierung verbindet

## Quantenalgorithmen als Builtins

Quantenalgorithmen sind native Primitive der Sprache — der Compiler gibt die Schaltung als Maschinencode aus:

* `grover(N)` — Grovers Suchalgorithmus
* `qft(N)` — Quantenfouriertransformation
* `shor(a, N)` — Shors Faktorisierungsalgorithmus
* `deutsch_jozsa(N)` — Deutsch-Jozsa-Algorithmus
* `bernstein_vazirani(N)` — Bernstein-Vazirani-Algorithmus

Ohne Qiskit. Ohne externe Abhängigkeiten.

## Laufzeit-Abhängigkeiten

**Keine.** Die resultierende Adsum-Binärdatei verlinkt gegen keine Drittanbieter-Bibliotheken:

* Ohne libsodium
* Ohne libcurl
* Ohne libc
* Ohne OpenSSL
* Ohne Qiskit

Der Compiler selbst ist in Python geschrieben und erfordert Python 3.x zur Ausführung.

## Verfügbare Architekturen

| Plattform         | Status          |
|-------------------|-----------------|
| x86_64 Linux       | Verfügbar v1.9.0 |
| x86_64 Windows     | Verfügbar v1.9.0 |
| ESP32-S3 bare metal | Verfügbar v1.9.0 |
| AArch64 Linux      | Verfügbar v1.9.0 |

## Lizenz

GEMÄSS DER ENDBENUTZER-LIZENZVEREINBARUNG (EULA) — ADSUM 1.x

---

# Adsum Programlama Dili

İnsanlar için programlama dili. Doğduğu andan itibaren yeni paradigmalar için tasarlandı: Blokzincir, IoT, Robotik ve Sinir Sistemleri.

## Neden Adsum?

Mevcut diller 20. yüzyıl paradigmaları için tasarlandı, ancak dünya değişti ve bugün merkezi olmayan sistemler, gömülü cihazlar, sinir mimarileri ve heterojen donanım geliştiriyoruz. Adsum bu yeni paradigmalar için doğdu — bir adaptasyon olarak değil, bir temel olarak.

## Felsefe

* **Kaynak kodda gerçek çok dillilik.** Dil, kültür, konum veya ülke fark etmeksizin anlaşılabilir, insana yakın ve erişilebilir olmalıdır.
* **Güçlü tip sistemi ve anlamsal olarak belirleyici.**
* **Doğrudan makine kodu üretimi.** Kaynakları boşa harcayan soyutlama katmanları olmadan.
* **Kriptografi, blokzincir, ağ ve yapay zeka için yerel ilkeller.** Kütüphaneler olarak değil, çekirdeğin temel parçası olarak.

## Proje Durumu

Mevcut sürüm: **1.9.0**

## Derleyici Mimarisi

Adsum, eksiksiz bir uçtan uca derleyicidir: herhangi bir insan dilinde kaynak kodu → dört mimaride statik çalıştırılabilir ikili. LLVM, GCC, ayrıştırıcı oluşturucuları veya çalışma zamanında üçüncü taraf kütüphaneleri olmadan.

10 aşamalı boru hattı:

```
Kaynak kodu (.adsum, herhangi bir insan dili)
  ↓
AŞAMA 0  — Çok dilli normalizasyon          
  ↓
AŞAMA 1  — Ön işleme (@resource)            
  ↓
AŞAMA 2  — Lexer + Parser → AST            
  ↓
AŞAMA 3  — Resmi Sözleşme Yükleme          
  ↓
AŞAMA 4  — Tip Kontrolü                     
  ↓
AŞAMA 5  — Bütünlük Koruyucu               
  ↓
AŞAMA 6  — Resmi Koruyucu                  
             ├── Ayrım Mantığı             
             └── Bölge Çözücü              
  ↓
AŞAMA 7  — Çerçeve Oluşturucu + Codegen ISA  
  ↓
AŞAMA 8  — Derleme                           
  ↓
AŞAMA 9  — Bağlama                           
  ↓
Statik çalıştırılabilir ikili
```

## Derleme Hedefleri

Aynı kaynak kodu, programı değiştirmeden herhangi bir hedefe derlenir:

| Format  | Platform          |
|---------|-------------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 bare metal |
| ELF64 AArch64 | AArch64 Linux  |

## Gerçek Çok Dillilik

Adsum 15 programlama dilini destekler:

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## Dilsel Egemenlik

Adsum belgeleri çevirmez, dilin kendisini çevirir. Kod, semantiği veya iç temsili değiştirmeden programcının ana dilinde yazılabilir. Tarafsız IR, tek resmi sözleşme olarak işlev görür ve tüm dilsel varyantların aynı ikili sonuca derlenmesini garanti eder; bu da şunları sağlar:

* Yazılım geliştirmede kültürel bağımsızlık
* İngilizce'ye bağımlı olmadan teknik erişim
* Diller arasında anlamsal tekdüzelik
* Yeni insan dillerine ölçeklenebilirlik

## Resmi Sözleşme (IR)

`system_manifest.json`, derleyicinin sözleşmesel ara temsili olarak işlev görür. Aynı anda şunları parametrelendiren tek gerçek kaynağıdır:

* **TypeChecker** — tüm yerleşiklerin tip imzaları
* **SeparationLogicVerifier** — bellek alanları ve çapraz alan kuralları (CDR'ler)
* **CodeGen** — ABI yazmaçları ve mimari başına `inject_*` fonksiyonları

`ManifestLoader` bu sözleşmeyi yükler, 6 katı değişmezi doğrular ve üç tüketiciye dağıtır. Hiçbir modül imzaları, alanları veya yazmaçları sabit kodlamaz — her şey manifestten türetilir.

## Kriptografik Egemenlik

Adsum'un kriptografik yığını libsodium ve OpenSSL'yi tamamen değiştirir ve ilk ilkelerden uygular:

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** sign, verify, keygen (RFC 8032)
* **ChaCha20** encrypt/decrypt (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** sabit zamanlı sonlu alan aritmetiği
* **Ed25519 eğrisi** dalsız Montgomery merdiveni ile (CMOV)

Tüm kriptografik işlemler yerel makine kodu olarak yayılır. spec/emit mimarisi, algoritmik belirtimi yayım arka ucundan ayırır ve çoklu mimari desteği sağlar.

## Yerel Ağ

TCP/IP ve HTTP ağı, işletim sistemi sistem çağrıları ile doğrudan uygulandı:

* **DNS** — Yerel çözünürlük (UDP port 53), `/etc/resolv.conf` okur
* **TCP** — libc olmadan connect, send, recv, close
* **HTTP** — Başlık oluşturucu ile GET/POST
* **Traceroute** — IP rota izleme (ICMP TTL)
* **ERFR** — Düğüm bağlamı ile kendi ağ protokolü

## IaG — Gerçek Yapay Zeka

AND, POPCNT, SHR ve CMP talimatlarına derlenen biyolojik olarak ilham alınmış sinir modeli. Kayan nokta yok, matris yok, geri yayılım yok.

* **Nöron** — 28 bit akson, 7 bit kapılı 4 Schwann hücresi
* **İşleme** — Schwann filtreler (AND) ve frekansa dönüştürür (POPCNT), dendrit eşikle karşılaştırır
* **Topoloji** — Ortaya çıkan aşamalarla tersine döndürülmüş Pascal üçgeni
* **İçgüdüler** — JSON'da önceden kablolu davranışlar (savunmacı, değerlendirici, proaktif, sosyal)
* **Bellek** — Duyusal (1 tick), kısa vadeli (16 anlık görüntü), uzun vadeli (bozunma ile 256 desen)
* **Yerel ilkeller** — axon_cargar, dendrita_disparar, udp_crear, json_a_texto dil yerleşikleri olarak kullanılabilir

## Resmi Doğrulama

Çöp toplayıcı olmadan bellek güvenliğini garanti eden matematiksel bariyer:

* **Ayrım Mantığı** — Bellek alanlarının (SAFE_STACK, IO_VOLATILE, IA_SENSORY, CRYPTO_SECURE, IOT_REALTIME, QUANTUM) ayrık olduğunu kanıtlar
* **Bölge Çözücü** — Statik kaçış analizi: sözcüksel kapsama, sarkık işaretçi algılama, yerel referansların kaçışını önleme
* **Bütünlük Koruyucu** — Null bütünlük, açık değiştirilebilirlik, `peligroso` blok kontrolü
* **Çapraz Alan Kuralları** — Açık `peligroso` bloğu olmadan alanlar arası kirliliği önleyen CDR'ler
* **ProgramProfile** — FormalGuard, CodeGen'e giden mühürlü bir profil üretir, statik analizi makine kodu üretimiyle bağlar

## Yerleşik Kuantum Algoritmalar

Kuantum algoritmaları dilin yerel ilkelleridir — derleyici devreyi makine kodu olarak yayar:

* `grover(N)` — Grover arama algoritması
* `qft(N)` — Kuantum Fourier Dönüşümü
* `shor(a, N)` — Shor çarpanlara ayırma algoritması
* `deutsch_jozsa(N)` — Deutsch-Jozsa algoritması
* `bernstein_vazirani(N)` — Bernstein-Vazirani algoritması

Qiskit yok. Harici bağımlılık yok.

## Çalışma Zamanı Bağımlılıkları

**Hiçbiri.** Ortaya çıkan Adsum ikilisi hiçbir üçüncü taraf kütüphanesiyle bağlantı kurmaz:

* libsodium yok
* libcurl yok
* libc yok
* OpenSSL yok
* Qiskit yok

Derleyicinin kendisi Python ile yazılmıştır ve çalıştırmak için Python 3.x gerektirir.

## Mevcut Mimariler

| Platform         | Durum          |
|------------------|----------------|
| x86_64 Linux       | Mevcut v1.9.0 |
| x86_64 Windows     | Mevcut v1.9.0 |
| ESP32-S3 bare metal | Mevcut v1.9.0 |
| AArch64 Linux      | Mevcut v1.9.0 |

## Lisans

SON KULLANICI LİSANS SÖZLEŞMESİNE (EULA) GÖRE — ADSUM 1.x

---

# Adsum 프로그래밍 언어

인간을 위한 프로그래밍 언어. 탄생부터 새로운 패러다임을 위해 설계되었습니다: 블록체인, IoT, 로보틱스, 신경 시스템.

## 왜 Adsum인가?

현재 언어들은 20세기 패러다임을 위해 설계되었지만, 세상은 변했고 오늘날 우리는 분산 시스템, 임베디드 장치, 신경 아키텍처, 이기종 하드웨어를 개발합니다. Adsum은 이러한 새로운 패러다임을 위해 탄생했습니다 — 적응이 아닌 토대로서.

## 철학

* **소스 코드에서의 진정한 다중언어.** 언어는 문화, 위치, 국가에 관계없이 이해하기 쉽고, 인간에 가깝고, 접근 가능해야 합니다.
* **강력한 타입 시스템과 의미론적으로 결정론적.**
* **직접적인 머신 코드 생성.** 리소스를 낭비하는 추상화 레이어 없음.
* **암호화, 블록체인, 네트워크, AI를 위한 네이티브 기본 요소.** 라이브러리가 아닌 코어의 기본 부분으로.

## 프로젝트 상태

현재 버전: **1.9.0**

## 컴파일러 아키텍처

Adsum은 완전한 엔드투엔드 컴파일러입니다: 모든 인간 언어의 소스 코드 → 4개 아키텍처의 정적 실행 바이너리. LLVM, GCC, 파서 생성기, 런타임 서드파티 라이브러리 없음.

10단계 파이프라인:

```
소스 코드 (.adsum, 모든 인간 언어)
  ↓
단계 0  — 다중언어 정규화          
  ↓
단계 1  — 전처리 (@resource)       
  ↓
단계 2  — Lexer + Parser → AST    
  ↓
단계 3  — 공식 계약 로드           
  ↓
단계 4  — 타입 검사               
  ↓
단계 5  — 무결성 가드              
  ↓
단계 6  — 형식 가드               
             ├── 분리 논리        
             └── 영역 해결사      
  ↓
단계 7  — 프레임 빌더 + 코드젠 ISA  
  ↓
단계 8  — 어셈블링               
  ↓
단계 9  — 링킹                   
  ↓
정적 실행 바이너리
```

## 컴파일 대상

동일한 소스 코드가 프로그램을 수정하지 않고 모든 대상으로 컴파일됩니다:

| 형식  | 플랫폼          |
|-------|-----------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 베어 메탈 |
| ELF64 AArch64 | AArch64 Linux  |

## 진정한 다중언어

Adsum은 15개 프로그래밍 언어를 지원합니다:

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## 언어적 주권

Adsum은 문서를 번역하는 것이 아니라 언어 자체를 번역합니다. 코드는 의미론이나 내부 표현을 변경하지 않고 프로그래머의 모국어로 작성될 수 있습니다. 중립적인 IR은 유일한 공식 계약으로 기능하여 모든 언어 변형이 동일한 바이너리 결과로 컴파일되도록 보장하며, 이는 다음을 가능하게 합니다:

* 소프트웨어 개발에서의 문화적 독립성
* 영어 의존 없는 기술적 접근
* 언어 간 의미론적 균일성
* 새로운 인간 언어로의 확장성

## 공식 계약 (IR)

`system_manifest.json`은 컴파일러의 계약적 중간 표현으로 기능합니다. 이것은 다음을 동시에 매개변수화하는 유일한 진실의 원천입니다:

* **TypeChecker** — 모든 내장 함수의 타입 시그니처
* **SeparationLogicVerifier** — 메모리 도메인과 크로스 도메인 규칙 (CDR)
* **CodeGen** — ABI 레지스터와 아키텍처별 `inject_*` 함수

`ManifestLoader`는 이 계약을 로드하고, 6개의 엄격한 불변량을 검증하며, 세 소비자에게 배포합니다. 어떤 모듈도 시그니처, 도메인, 레지스터를 하드코딩하지 않습니다 — 모든 것은 매니페스트에서 파생됩니다.

## 암호화 주권

Adsum의 암호화 스택은 libsodium과 OpenSSL을 완전히 대체하며, 첫 번째 원칙부터 구현합니다:

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** sign, verify, keygen (RFC 8032)
* **ChaCha20** encrypt/decrypt (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** 상수 시간 유한체 산술
* **Ed25519 곡선** 브랜치리스 Montgomery 래더 (CMOV) 포함

모든 암호화 연산은 네이티브 머신 코드로 출력됩니다. spec/emit 아키텍처는 알고리즘 사양을 출력 백엔드에서 분리하여 멀티 아키텍처 지원을 가능하게 합니다.

## 네이티브 네트워킹

OS 시스템 콜로 직접 구현된 TCP/IP 및 HTTP 네트워크:

* **DNS** — 네이티브 해석 (UDP 포트 53), `/etc/resolv.conf` 읽기
* **TCP** — libc 없이 connect, send, recv, close
* **HTTP** — 헤더 빌더가 있는 GET/POST
* **Traceroute** — IP 경로 추적 (ICMP TTL)
* **ERFR** — 노드 컨텍스트가 있는 자체 네트워크 프로토콜

## IaG — 진정한 인공 지능

AND, POPCNT, SHR, CMP 명령어로 컴파일되는 생물학적으로 영감받은 신경 모델. 부동소수점 없음, 행렬 없음, 역전파 없음.

* **뉴런** — 28비트 축삭, 7비트 게이트가 있는 4개의 슈반 세포
* **처리** — 슈반이 필터링 (AND)하고 주파수로 변환 (POPCNT), 수상돌기가 임계값과 비교
* **토폴로지** — 창발적 단계가 있는 역전된 파스칼 삼각형
* **본능** — JSON에 사전 배선된 행동 (방어적, 평가적, 능동적, 사회적)
* **메모리** — 감각적 (1 틱), 단기 (16 스냅샷), 장기 (감쇠가 있는 256 패턴)
* **네이티브 기본 요소** — axon_cargar, dendrita_disparar, udp_crear, json_a_texto가 언어 내장 함수로 제공

## 형식 검증

가비지 컬렉터 없이 메모리 안전성을 보장하는 수학적 장벽:

* **분리 논리** — 메모리 도메인 (SAFE_STACK, IO_VOLATILE, IA_SENSORY, CRYPTO_SECURE, IOT_REALTIME, QUANTUM)이 분리되어 있음을 증명
* **영역 해결사** — 정적 탈출 분석: 어휘적 격리, 댕글링 포인터 감지, 로컬 참조의 탈출 방지
* **무결성 가드** — Null 무결성, 명시적 가변성, `peligroso` 블록 제어
* **크로스 도메인 규칙** — 명시적 `peligroso` 블록 없이 도메인 간 오염을 방지하는 CDR
* **ProgramProfile** — FormalGuard가 CodeGen으로 전달되는 봉인된 프로파일을 생성하여 정적 분석과 머신 코드 생성을 연결

## 내장 함수로서의 양자 알고리즘

양자 알고리즘은 언어의 네이티브 기본 요소입니다 — 컴파일러가 회로를 머신 코드로 출력합니다:

* `grover(N)` — Grover 검색 알고리즘
* `qft(N)` — 양자 푸리에 변환
* `shor(a, N)` — Shor 소인수분해 알고리즘
* `deutsch_jozsa(N)` — Deutsch-Jozsa 알고리즘
* `bernstein_vazirani(N)` — Bernstein-Vazirani 알고리즘

Qiskit 없음. 외부 의존성 없음.

## 런타임 의존성

**없음.** 생성된 Adsum 바이너리는 어떤 서드파티 라이브러리와도 링크하지 않습니다:

* libsodium 없음
* libcurl 없음
* libc 없음
* OpenSSL 없음
* Qiskit 없음

컴파일러 자체는 Python으로 작성되었으며 실행에 Python 3.x가 필요합니다.

## 사용 가능한 아키텍처

| 플랫폼         | 상태          |
|----------------|---------------|
| x86_64 Linux       | 사용 가능 v1.9.0 |
| x86_64 Windows     | 사용 가능 v1.9.0 |
| ESP32-S3 베어 메탈 | 사용 가능 v1.9.0 |
| AArch64 Linux      | 사용 가능 v1.9.0 |

## 라이선스

최종 사용자 라이선스 계약 (EULA)에 따라 — ADSUM 1.x

---

# Linguaggio Adsum

Il linguaggio di programmazione per gli esseri umani. Progettato sin dalla nascita per i nuovi paradigmi: Blockchain, IoT, Robotica e Sistemi Neurali.

## Perché Adsum?

I linguaggi attuali sono stati progettati per i paradigmi del ventesimo secolo, ma il mondo è cambiato e oggi sviluppiamo sistemi decentralizzati, dispositivi incorporati, architetture neurali e hardware eterogeneo. Adsum nasce per questi nuovi paradigmi — non come adattamento, ma come fondamento.

## Filosofia

* **Vera multilingualità nel codice sorgente.** Il linguaggio deve essere comprensibile, vicino all'umano e accessibile indipendentemente dalla tua cultura, posizione o paese.
* **Tipizzazione forte e semanticamente deterministica.**
* **Generazione diretta di codice macchina.** Senza strati di astrazione che sprecano risorse.
* **Primitive native per crittografia, blockchain, rete e IA.** Come parte fondamentale del nucleo, non come librerie.

## Stato del Progetto

Versione attuale: **1.9.0**

## Architettura del Compilatore

Adsum è un compilatore completo end-to-end: codice sorgente in qualsiasi lingua umana → binario statico eseguibile in quattro architetture. Senza LLVM, GCC, generatori di parser, né librerie di terze parti in runtime.

Pipeline di 10 fasi:

```
Codice sorgente (.adsum, qualsiasi lingua umana)
  ↓
FASE 0  — Normalizzazione multilingue          
  ↓
FASE 1  — Preelaborazione (@resource)          
  ↓
FASE 2  — Lexer + Parser → AST               
  ↓
FASE 3  — Caricamento del Contratto Formale   
  ↓
FASE 4  — Verifica dei Tipi                  
  ↓
FASE 5  — Integrity Guard                     
  ↓
FASE 6  — Formal Guard                        
             ├── Separation Logic             
             └── Region Solver               
  ↓
FASE 7  — Frame Builder + Codegen ISA        
  ↓
FASE 8  — Assemblaggio                        
  ↓
FASE 9  — Collegamento                        
  ↓
Binario statico eseguibile
```

## Target di Compilazione

Lo stesso codice sorgente viene compilato per qualsiasi target senza modificare il programma:

| Formato  | Piattaforma          |
|----------|----------------------|
| ELF64    | x86_64 Linux        |
| PE32+    | x86_64 Windows      |
| ELF32 Xtensa | ESP32-S3 bare metal |
| ELF64 AArch64 | AArch64 Linux  |

## Vera Multilingualità

Adsum supporta 15 linguaggi di programmazione:

* Español
* English
* 中文 (Chinese)
* हिन्दी (Hindi)
* العربية (Arabic)
* বাংলা (Bengali)
* Português
* Русский (Russian)
* 日本語 (Japanese)
* Bahasa Indonesia
* Français
* Deutsch
* Türkçe
* 한국어 (Korean)
* Italiano

## Sovranità Linguistica

Adsum non traduce la documentazione, traduce il linguaggio stesso. Il codice può essere scritto nella lingua madre del programmatore senza alterare la semantica né la rappresentazione interna. L'IR neutro agisce come unico contratto formale, garantendo che tutte le varianti linguistiche vengano compilate nello stesso risultato binario, consentendo:

* Indipendenza culturale nello sviluppo del software
* Accesso tecnico senza dipendenza dall'inglese
* Uniformità semantica tra le lingue
* Scalabilità verso nuove lingue umane

## Contratto Formale (IR)

Il `system_manifest.json` agisce come rappresentazione intermedia contrattuale del compilatore. È la fonte unica di verità che parametrizza simultaneamente:

* **TypeChecker** — firme di tipo di tutti i builtin
* **SeparationLogicVerifier** — domini di memoria e regole cross-domain (CDR)
* **CodeGen** — registri ABI e funzioni `inject_*` per architettura

Il `ManifestLoader` carica questo contratto, valida 6 invarianti strette e lo distribuisce ai tre consumatori. Nessun modulo hardcodifica firme, domini o registri — tutto deriva dal manifest.

## Sovranità Crittografica

Lo stack crittografico di Adsum sostituisce completamente libsodium e OpenSSL, implementando dai primi principi:

* **SHA-256 / SHA-512** (FIPS 180-4)
* **Ed25519** sign, verify, keygen (RFC 8032)
* **ChaCha20** encrypt/decrypt (RFC 8439)
* **HMAC-SHA256** (RFC 2104)
* **GF(2²⁵⁵−19)** aritmetica su campo finito in tempo costante
* **Curva Ed25519** con Montgomery ladder branchless (CMOV)

Tutte le operazioni crittografiche vengono emesse come codice macchina nativo. L'architettura spec/emit separa la specifica algoritmica dal backend di emissione, abilitando il supporto multi-architettura.

## Networking Nativo

Rete TCP/IP e HTTP implementate direttamente con syscall del sistema operativo:

* **DNS** — Risoluzione nativa (UDP porta 53), legge `/etc/resolv.conf`
* **TCP** — connect, send, recv, close senza libc
* **HTTP** — GET/POST con costruttore di header
* **Traceroute** — Tracciamento percorso IP (ICMP TTL)
* **ERFR** — Protocollo di rete proprio con contesto di nodo

## IaG — Intelligenza Artificiale Genuina

Modello neurale biologicamente ispirato che compila in istruzioni AND, POPCNT, SHR e CMP. Senza virgola mobile, senza matrici, senza backpropagation.

* **Neurone** — 28 bit di assoni, 4 cellule di Schwann con porte a 7 bit
* **Elaborazione** — Schwann filtra (AND) e converte in frequenza (POPCNT), dendrite confronta con la soglia
* **Topologia** — Triangolo di Pascal invertito con fasi emergenti
* **Istinti** — Comportamenti pre-cablati in JSON (difensivi, valutativi, proattivi, sociali)
* **Memoria** — Sensoriale (1 tick), breve termine (16 snapshot), lungo termine (256 pattern con decadimento)
* **Primitive native** — axon_cargar, dendrita_disparar, udp_crear, json_a_texto disponibili come builtin del linguaggio

## Verifica Formale

Barriera matematica che garantisce la sicurezza della memoria senza garbage collector:

* **Separation Logic** — Dimostra che i domini di memoria (SAFE_STACK, IO_VOLATILE, IA_SENSORY, CRYPTO_SECURE, IOT_REALTIME, QUANTUM) sono disgiunti
* **Region Solver** — Analisi di escape statica: contenimento lessicale, rilevamento di puntatori pendenti, prevenzione dell'escape di riferimenti locali
* **Integrity Guard** — Null integrity, mutabilità esplicita, controllo dei blocchi `peligroso`
* **Cross-Domain Rules** — CDR che impediscono la contaminazione tra domini senza blocco `peligroso` esplicito
* **ProgramProfile** — Il FormalGuard produce un profilo sigillato che viaggia al CodeGen, collegando l'analisi statica alla generazione di codice macchina

## Algoritmi Quantistici come Builtin

Gli algoritmi quantistici sono primitive native del linguaggio — il compilatore emette il circuito come codice macchina:

* `grover(N)` — Algoritmo di ricerca di Grover
* `qft(N)` — Trasformata di Fourier Quantistica
* `shor(a, N)` — Algoritmo di fattorizzazione di Shor
* `deutsch_jozsa(N)` — Algoritmo Deutsch-Jozsa
* `bernstein_vazirani(N)` — Algoritmo Bernstein-Vazirani

Senza Qiskit. Senza dipendenze esterne.

## Dipendenze in Runtime

**Nessuna.** Il binario Adsum risultante non si collega ad alcuna libreria di terze parti:

* Senza libsodium
* Senza libcurl
* Senza libc
* Senza OpenSSL
* Senza Qiskit

Il compilatore stesso è scritto in Python e richiede Python 3.x per essere eseguito.

## Architetture Disponibili

| Piattaforma         | Stato          |
|---------------------|----------------|
| x86_64 Linux       | Disponibile v1.9.0 |
| x86_64 Windows     | Disponibile v1.9.0 |
| ESP32-S3 bare metal | Disponibile v1.9.0 |
| AArch64 Linux      | Disponibile v1.9.0 |

## Licenza

SECONDO L'ACCORDO DI LICENZA CON L'UTENTE FINALE (EULA) — ADSUM 1.x

# Awesome-JSON-Parsers-Utilities

# Top JSON Parsers & Utilities Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on High-Performance Parsing, Language-Native Serialization & Self-Hosted Data Interchange*  
**Last updated: October 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **JSON Parsers & Utilities**. These libraries provide the fundamental capability of reading and writing JSON—the universal data interchange format of the web—with varying trade-offs in speed, memory efficiency, API ergonomics, and language integration.

**Examples** include simdjson, yyjson, RapidJSON, nlohmann/json, cJSON, Jackson, Gson, fastjson, Oj, and Serde JSON (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project across C/C++, Java, Ruby, and Rust—ideal for systems programmers, backend engineers, and anyone who needs to parse JSON without paying for a hosted service or accepting a vendor's abstraction. The open-source landscape is anchored by **simdjson** (the SIMD-accelerated throughput champion), **yyjson** (the ANSI C speed and correctness leader), **nlohmann/json** (the ergonomic C++ standard), **Jackson** (the Java ecosystem default), and **Serde** (the Rust serialization framework), with **cJSON** serving embedded systems and **Oj** serving Ruby performance needs.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **No dedicated SaaS platforms exist for JSON parsing.** These libraries run in-process—their purpose is to transform data within your application, not to provide a hosted service. Cloud APIs that accept or return JSON use these open-source libraries internally; they are not JSON parsing services themselves.

## Open-Source GitHub Projects

### C / C++

- **[simdjson](https://github.com/simdjson/simdjson)**  
  **The SIMD-accelerated throughput champion.** Uses parallel instruction sets (AVX2, NEON) to parse JSON at gigabytes per second—roughly **4 GB/s** in benchmarks, compared to ~400 MB/s for DOM-based alternatives . **On-demand parsing** allows accessing specific fields without materializing the entire document. **Important caveat**: benchmark data shows simdjson returned undefined results on more test cases than yyjson or RapidJSON in correctness testing . Best for large-file processing and analytics pipelines where raw speed justifies the API complexity.

- **[yyjson](https://github.com/ibireme/yyjson)**  
  **The ANSI C speed and correctness leader.** Written in portable C with no dependencies. **Benchmark data**: streaming parse of a 48 MB file in **~317 ms** vs. yajl2's ~500 ms; 283 of 283 correctness tests passed with 0 false successes or failures . **Strengths**: excellent correctness record, clean C API, and competitive performance without SIMD complexity. Best for C/C++ projects that want high performance with a simpler integration than simdjson.

- **[RapidJSON](https://github.com/Tencent/rapidjson)**  
  **The mature C++ alternative.** Header-only, fast, and widely used. **Trade-offs**: correctness testing shows 277 of 283 expected results, with some edge cases producing undefined behavior . API is lower-level than nlohmann/json. Best for projects already using it or needing header-only C++ with good performance.

- **[nlohmann/json](https://github.com/nlohmann/json)**  
  **The ergonomic C++ standard.** The most widely used C++ JSON library—excellent for rapid prototyping, small data, and schema-less exploration . **Strengths**: clean, readable API; `NLOHMANN_DEFINE_TYPE_NON_INTRUSIVE` macro generates serialization code without touching struct definitions; `.value("key", default)` returns defaults for missing fields cleanly . **Trade-offs**: significantly slower (~400 MB/s) and higher allocation overhead than simdjson or yyjson . Best for everyday C++ JSON work where developer time matters more than parse speed.

- **[cJSON](https://github.com/DaveGamble/cJSON)**  
  **The embedded systems standard.** A single C file and single header—"the dumbest possible parser that you can get your job done with" . **Strengths**: zero dependencies, trivial to integrate into any C project (including microcontrollers), and a simple struct-based API (`cJSON_Parse`, `cJSON_GetObjectItem`). **Trade-offs**: no performance optimization, manual memory management, and limited features compared to modern alternatives . Best for embedded systems, firmware, and C projects where minimal footprint is the primary constraint.

### Java

- **[Jackson](https://github.com/FasterXML/jackson)**  
  **The Java ecosystem default.** Spring Boot's default JSON processor, with three processing modes: **Streaming API** (Stax-inspired, highest performance), **Tree Model** (DOM-like), and **Data Binding** (POJO to/from JSON) . **Strengths**: deep Spring integration, extensive module ecosystem, and mature handling of generics and custom serializers. **Trade-offs**: annotation-heavy API; steeper learning curve than Gson . Best for Spring Boot applications and enterprise Java where ecosystem compatibility matters most.

- **[Gson](https://github.com/google/gson)**  
  **The simple Java alternative.** Designed to convert arbitrary Java objects (including pre-existing classes without source access) to JSON without requiring annotations . **Strengths**: `toJson()` and `fromJson()` are the entire API surface for most use cases; excellent generics support . **Status note**: Gson is in **maintenance mode**—existing bugs will be fixed, but large new features are unlikely . Best for simple Java projects that want minimal configuration and don't need Jackson's ecosystem depth.

- **[fastjson](https://github.com/alibaba/fastjson)**  
  **The performance-focused Chinese ecosystem standard.** Slightly faster than Jackson for small objects, with a simpler API and Chinese documentation . **Critical security caveat**: historical versions (<1.2.68) had severe deserialization vulnerabilities; **Fastjson 2.x <= 2.0.62 has a CVSS 9.8 remote code execution vulnerability** disclosed in 2026 . **SafeMode** (introduced in 1.2.68) disables autoType and mitigates deserialization attacks. **Recommendation**: use only if your team is already familiar with it and you run the latest patched version with SafeMode enabled; otherwise prefer Jackson for long-term maintainability . Best for teams within the Alibaba ecosystem that understand the security posture required.

### Ruby

- **[Oj](https://github.com/ohler55/oj)**  
  **The Ruby performance standard.** A fast JSON parser and object marshaller with optimized C extensions . **Version 3.13** added a much faster parser (`Oj::Parser`) and option isolation. **Strengths**: significantly faster than Ruby's stdlib JSON; supports Rails/ActiveSupport compatibility modes; flexible options for different use cases . Best for Ruby/Rails applications where JSON parsing is a measurable bottleneck.

### Rust

- **[Serde JSON](https://github.com/serde-rs/json)**  
  **The Rust serialization framework.** Part of the broader Serde ecosystem, which uses Rust's trait system rather than runtime reflection—the compiler can often optimize serialization away entirely, matching handwritten serializer speed . **Strengths**: `#[derive(Serialize, Deserialize)]` generates implementations at compile time; works across JSON, YAML, TOML, and many other formats with the same data structures . **Trade-offs**: compile-time errors for schema mismatches are the Rust way—different from dynamic language flexibility. Best for any Rust project that needs JSON, which is effectively all of them.

### Additional Strong Open-Source Options

- **ijson** — Streaming/SAX-style Python JSON parser. Best for processing JSON too large to fit in memory .
- **glaze** — C++20 compile-time reflection library with parse speed close to simdjson (~2 GB/s) and far better ergonomics . Best for modern C++ projects wanting speed and type safety.
- **Frameworks for building custom systems**: Use **simdjson** or **yyjson** for C/C++ throughput-critical parsing. Use **nlohmann/json** for C++ ergonomics. Use **Jackson** for Java enterprise, **Gson** for simple Java, **Serde** for Rust, **Oj** for Ruby. Use **cJSON** for embedded C. Avoid **fastjson** without SafeMode and a security review.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- **Benchmark performance is highly task-dependent**: published figures for the same libraries vary by 2–3x depending on data shape, access pattern, and hardware . Use the benchmarks as relative guidance, not absolute guarantees.
- **Correctness matters as much as speed**: simdjson's undefined results in correctness testing suggest that raw throughput claims should be validated against your actual data schema . yyjson's clean correctness record makes it a safer default for untrusted input.
- **fastjson has a recent critical vulnerability**: CVSS 9.8 RCE in 2.x <= 2.0.62. If you must use it, upgrade immediately and enable SafeMode. For new projects, Jackson is the safer long-term choice .
- **Gson is in maintenance mode**: it will receive bug fixes but not major new features. Evaluate whether your project needs active feature development before choosing it .

---

**Made for systems programmers, backend engineers, and developers who need to parse JSON correctly and efficiently.**  
Let's make JSON parsing more open, appropriately scoped, and security-conscious.

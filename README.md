# Campus Network Infrastructure Project 🌐

هذا المستودع يحتوي على ملفات التكوين لشبكة مؤسسية تعتمد على التصميم الهرمي ثلاثي الطبقات (**Three-Tier Architecture**)، مع التركيز على توزيع الأحمال الذكي (Load Sharing) في طبقة التوزيع.

---

## 🏗️ مخطط الشبكة (Network Topology)

تعتمد الشبكة على تصميم **Hierarchical Model** لضمان استقرار الخدمة وتوزيع المهام بين الأجهزة:

![Network Design](Three-Tier.png)

---

## 🛠️ التقنيات والبروتوكولات (Tech Stack)

### **High Availability & Load Sharing**
* **VRRP Active-Active:** توزيع الأدوار بحيث يعمل كل جهاز كبوابة رئيسية (Primary Gateway) لمجموعة محددة من الـ VLANs وبوابة احتياطية للمجموعة الأخرى.
* **VLAN-Based Spanning Tree (MST/PVST):** تخصيص أولوية الـ **Root Bridge** لكل VLAN بشكل مستقل، لضمان تطابق مسار الطبقة الثانية (L2 Path) مع مخرج الطبقة الثالثة (L3 Gateway).
* **Rapid-PVST+:** تسريع عملية التقارب (Convergence) ومنع الحلقات في الشبكة.
* **VTP v3:** إدارة مركزية وآمنة لقواعد بيانات الـ VLANs عبر نطاقات (Domains) منفصلة.

### **Routing & Services**
* **Multi-Area OSPF:** ربط طبقة التوزيع بالـ Core عبر Area 0 و Area 1 و Area 2 لعزل ترافيك الأقسام.
* **LACP EtherChannel:** تجميع الروابط لزيادة السرعة بين السويتشات وتوفير مسارات بديلة.
* **DHCP Pools:** توزيع العناوين محلياً على مستوى طبقة التوزيع لتقليل التأخير.

---

## 📁 توزيع المهام والتحكم في المسارات (Distribution Layer Matrix)

| الجهاز | القطاع | الدور الوظيفي | الـ VLANs (Primary / Root) | STP Priority |
| :--- | :--- | :--- | :--- | :--- |
| **Distrib-1** | South | Multi-Service Gateway | Marketing & IT (10, 20) | 24576 |
| **Distrib-2** | South | Multi-Service Gateway | Account & HR (30, 40) | 24576 |
| **Distrib-3** | North | Multi-Service Gateway | Engineering & Research (50, 60) | 24576 |
| **Distrib-4** | North | Multi-Service Gateway | Logistics & Management (70, 80) | 24576 |

---

## 🚀 ملاحظات التوثيق

1. **Deterministic Pathing:** تم ضبط قيم الـ `Priority` في الـ STP والـ VRRP بشكل متزامن لضمان سلوك شبكة "محدد مسبقاً" وتجنب المسارات غير المثالية (Sub-optimal routing).
2. **Isolation:** فصل الشمال عن الجنوب منطقياً عبر VTP Domains مختلفة (abc & xyz) لتعزيز الأمان وتقليل نطاق بث الـ VLANs.
3. **Efficiency:** هذا التصميم يلغي وجود الأجهزة الخاملة، حيث تتم معالجة البيانات وتوزيعها عبر جميع الروابط المتاحة بالتوازي.

---

## 👨‍💻 إشراف المهندس
**تم تصميم وتوثيق هذه الشبكة لتعمل بأقصى كفاءة ممكنة مع ضمان توافر الخدمة بنسبة 99.99%.**

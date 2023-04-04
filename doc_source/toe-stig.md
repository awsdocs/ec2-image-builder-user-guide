# AWS Task Orchestrator and Executor STIG components<a name="toe-stig"></a>

Security Technical Implementation Guides \(STIGs\) are the configuration standards created by the Defense Information Systems Agency \(DISA\) to secure information systems and software\. To make your systems compliant with STIG standards, you must install, configure, and test a variety of security settings\.

AWS Task Orchestrator and Executor provides STIG components to help you more efficiently build compliant images for STIG standards\. These STIG components scan for misconfigurations and run a remediation script\. There are no additional charges for using STIG\-compliant components\.

**Compliance levels**
+ **High \(Category I\) **

  The most severe risk\. Includes any vulnerability that can result in loss of confidentiality, availability, or integrity\.
+ **Medium \(Category II\) **

  Includes any vulnerability that can result in loss of confidentiality, availability, or integrity, but the risks can be mitigated\.
+ **Low \(Category III\) **

  Any vulnerability that degrades measures to protect against loss of confidentiality, availability, or integrity\.

**Topics**
+ [Windows STIG components](#windows-os-stig)
+ [Linux STIG components](#linux-os-stig)
+ [SCAP compliance validator component](#scap-compliance)

## Windows STIG components<a name="windows-os-stig"></a>

Windows STIG components are designed for standalone servers and apply Local Group Policy\. STIG\-compliant components install InstallRoot from the Department of Defense \(DoD\) on Windows infrastructure to download, install, and update the DoD certificates\. They also remove unnecessary certificates to maintain STIG compliance\. Currently, STIG baselines are supported for the following versions of Windows Server: 2012 R2, 2016, 2019, and 2022\.

This section lists current settings for each of the Windows STIG components, followed by a version history log\.

### STIG\-Build\-Windows\-Low version 2022\.4\.0<a name="ib-windows-stig-low"></a>

The following list contains STIG settings that the component applies to your infrastructure\. If a setting isn't applicable for your infrastructure, the component skips that setting, and moves on\. For example, some STIG settings might not apply to standalone servers\. Organization\-specific policies can also affect which settings the component applies, such as a requirement for administrators to review document settings\. For more details about the STIGs that apply to Windows AMIs, you can download our [spreadsheet](https://aws-windows-downloads-us-west-1.s3.us-west-1.amazonaws.com/STIG/Win+STIG.xlsx)\.

For a complete list of Windows STIGs, see the [STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=windows)\. For information about how to view the complete list, see [STIG Viewing Tools](https://public.cyber.mil/stigs/srg-stig-tools/)\.
+ **Windows Server 2022 STIG Version 1 Release 1**

  V\-254335, V\-254336, V\-254337, V\-254338, V\-254351, V\-254357, V\-254363, and V\-254481
+ **Windows Server 2019 STIG Version 2 Release 5**

  V\-205691, V\-205819, V\-205858, V\-205859, V\-205860, V\-205870, V\-205871, and V\-205923
+ **Windows Server 2016 STIG Version 2 Release 5**

  V\-224916, V\-224917, V\-224918, V\-224919, V\-224931, V\-224942, and V\-225060
+ **Windows Server 2012 R2 MS STIG Version 3 Release 5**

  V\-225537, V\-225536, V\-225526, V\-225525, V\-225514, V\-225511, V\-225490, V\-225489, V\-225488, V\-225487, V\-225485, V\-225484, V\-225483, V\-225482, V\-225481, V\-225480, V\-225479, V\-225476, V\-225473, V\-225468, V\-225462, V\-225460, V\-225459, V\-225412, V\-225394, V\-225392, V\-225376, V\-225363, V\-225362, V\-225360, V\-225359, V\-225358, V\-225357, V\-225355, V\-225343, V\-225342, V\-225336, V\-225335, V\-225334, V\-225333, V\-225332, V\-225331, V\-225330, V\-225328, V\-225327, V\-225324, V\-225319, V\-225318, and V\-225250
+ **Microsoft \.NET Framework 4\.0 STIG Version 2 Release 2**

  No STIG settings are applied to the Microsoft \.NET Framework for Category III vulnerabilities\.
+ **Windows Firewall STIG Version 2 Release 1**

  V\-241994, V\-241995, V\-241996, V\-241999, V\-242000, V\-242001, V\-242006, V\-242007, and V\-242008
+ **Internet Explorer 11 STIG Version 2 Release 3**

  V\-46477, V\-46629, and V\-97527
+ **Microsoft Edge STIG Version 1 Release 6 \(Windows Server 2022 only\)**

  V\-235727, V\-235731, V\-235751, V\-235752, and V\-235765

### STIG\-Build\-Windows\-Medium version 2022\.4\.0<a name="ib-windows-stig-medium"></a>

The following list contains STIG settings that the component applies to your infrastructure\. If a setting isn't applicable for your infrastructure, the component skips that setting, and moves on\. For example, some STIG settings might not apply to standalone servers\. Organization\-specific policies can also affect which settings the component applies, such as a requirement for administrators to review document settings\. For more details about the STIGs that apply to Windows AMIs, you can download our [spreadsheet](https://aws-windows-downloads-us-west-1.s3.us-west-1.amazonaws.com/STIG/Win+STIG.xlsx)\.

For a complete list of Windows STIGs, see the [STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=windows)\. For information about how to view the complete list, see [STIG Viewing Tools](https://public.cyber.mil/stigs/srg-stig-tools/)\.

**Note**  
The STIG\-Build\-Windows\-Medium components include all STIG settings that AWSTOE applies for STIG\-Build\-Windows\-Low components, in addition to the STIG settings that apply specifically for Category II vulnerabilities\.
+ **Windows Server 2022 STIG Version 1 Release 1**

  V\-254247, V\-254265, V\-254269, V\-254270, V\-254271, V\-254272, V\-254273, V\-254274, V\-254276, V\-254277, V\-254278, V\-254285, V\-254286, V\-254287, V\-254288, V\-254289, V\-254290, V\-254291, V\-254292, V\-254300, V\-254301, V\-254302, V\-254303, V\-254304, V\-254305, V\-254306, V\-254307, V\-254308, V\-254309, V\-254310, V\-254311, V\-254312, V\-254313, V\-254314, V\-254315, V\-254316, V\-254317, V\-254318, V\-254319, V\-254320, V\-254321, V\-254322, V\-254323, V\-254324, V\-254325, V\-254326, V\-254327, V\-254328, V\-254329, V\-254330, V\-254331, V\-254332, V\-254333, V\-254334, V\-254339, V\-254341, V\-254342, V\-254344, V\-254345, V\-254346, V\-254347, V\-254348, V\-254349, V\-254350, V\-254355, V\-254356, V\-254358, V\-254359, V\-254360, V\-254361, V\-254362, V\-254364, V\-254365, V\-254366, V\-254367, V\-254368, V\-254369, V\-254370, V\-254371, V\-254372, V\-254373, V\-254375, V\-254376, V\-254377, V\-254379, V\-254380, V\-254382, V\-254383, V\-254431, V\-254432, V\-254433, V\-254434, V\-254435, V\-254436, V\-254438, V\-254439, V\-254442, V\-254443, V\-254444, V\-254445, V\-254449, V\-254450, V\-254451, V\-254452, V\-254453, V\-254454, V\-254455, V\-254456, V\-254459, V\-254460, V\-254461, V\-254462, V\-254463, V\-254464, V\-254468, V\-254470, V\-254471, V\-254472, V\-254473, V\-254476, V\-254477, V\-254478, V\-254479, V\-254480, V\-254482, V\-254483, V\-254484, V\-254485, V\-254486, V\-254487, V\-254488, V\-254489, V\-254490, V\-254493, V\-254494, V\-254495, V\-254497, V\-254499, V\-254501, V\-254502, V\-254503, V\-254504, V\-254505, V\-254507, V\-254508, V\-254509, V\-254510, V\-254511, and V\-254512
+ **Windows Server 2019 STIG Version 2 Release 5**

  Includes all STIG settings that AWSTOE applies for Category III \(Low\) vulnerabilities, plus:

  V\-205625, V\-205626, V\-205627, V\-205629, V\-205630, V\-205633, V\-205634, V\-205635, V\-205636, V\-205637, V\-205638, V\-205639, V\-205643, V\-205644, V\-205648, V\-205649, V\-205650, V\-205651, V\-205652, V\-205655, V\-205656, V\-205659, V\-205660, V\-205662, V\-205671, V\-205672, V\-205673, V\-205675, V\-205676, V\-205678, V\-205679, V\-205680, V\-205681, V\-205682, V\-205683, V\-205684, V\-205685, V\-205686, V\-205687, V\-205688, V\-205689, V\-205690, V\-205692, V\-205693, V\-205694, V\-205697, V\-205698, V\-205708, V\-205709, V\-205712, V\-205714, V\-205716, V\-205717, V\-205718, V\-205719, V\-205720, V\-205722, V\-205729, V\-205730, V\-205733, V\-205747, V\-205751, V\-205752, V\-205754, V\-205756, V\-205758, V\-205759, V\-205760, V\-205761, V\-205762, V\-205764, V\-205765, V\-205766, V\-205767, V\-205768, V\-205769, V\-205770, V\-205771, V\-205772, V\-205773, V\-205774, V\-205775, V\-205776, V\-205777, V\-205778, V\-205779, V\-205780, V\-205781, V\-205782, V\-205783, V\-205784, V\-205795, V\-205796, V\-205797, V\-205798, V\-205801, V\-205808, V\-205809, V\-205810, V\-205811, V\-205812, V\-205813, V\-205814, V\-205815, V\-205816, V\-205817, V\-205821, V\-205822, V\-205823, V\-205824, V\-205825, V\-205826, V\-205827, V\-205828, V\-205830, V\-205832, V\-205833, V\-205834, V\-205835, V\-205836, V\-205837, V\-205838, V\-205839, V\-205840, V\-205841, V\-205861, V\-205863, V\-205865, V\-205866, V\-205867, V\-205868, V\-205869, V\-205872, V\-205873, V\-205874, V\-205911, V\-205912, V\-205915, V\-205916, V\-205917, V\-205918, V\-205920, V\-205921, V\-205922, V\-205924, V\-205925, and V\-236001
+ **Windows Server 2016 STIG Version 2 Release 5**

  Includes all STIG settings that AWSTOE applies for Category III \(Low\) vulnerabilities, plus:

  V\-224850, V\-224852, V\-224853, V\-224854, V\-224855, V\-224856, V\-224857, V\-224858, V\-224859, V\-224866, V\-224867, V\-224868, V\-224869, V\-224870, V\-224871, V\-224872, V\-224873, V\-224881, V\-224882, V\-224883, V\-224884, V\-224885, V\-224886, V\-224887, V\-224888, V\-224889, V\-224890, V\-224891, V\-224892, V\-224893, V\-224894, V\-224895, V\-224896, V\-224897, V\-224898, V\-224899, V\-224900, V\-224901, V\-224902, V\-224903, V\-224904, V\-224905, V\-224906, V\-224907, V\-224908, V\-224909, V\-224910, V\-224911, V\-224912, V\-224913, V\-224914, V\-224915, V\-224920, V\-224922, V\-224924, V\-224925, V\-224926, V\-224927, V\-224928, V\-224929, V\-224930, V\-224935, V\-224936, V\-224937, V\-224938, V\-224939, V\-224940, V\-224941, V\-224943, V\-224944, V\-224945, V\-224946, V\-224947, V\-224948, V\-224949, V\-224951, V\-224952, V\-224953, V\-224955, V\-224956, V\-224957, V\-224959, V\-224960, V\-224962, V\-224963, V\-225010, V\-225013, V\-225014, V\-225015, V\-225016, V\-225017, V\-225018, V\-225019, V\-225021, V\-225022, V\-225023, V\-225024, V\-225028, V\-225029, V\-225030, V\-225031, V\-225032, V\-225033, V\-225034, V\-225035, V\-225038, V\-225039, V\-225040, V\-225041, V\-225042, V\-225043, V\-225047, V\-225049, V\-225050, V\-225051, V\-225052, V\-225055, V\-225056, V\-225057, V\-225058, V\-225061, V\-225062, V\-225063, V\-225064, V\-225065, V\-225066, V\-225067, V\-225068, V\-225069, V\-225072, V\-225073, V\-225074, V\-225076, V\-225078, V\-225080, V\-225081, V\-225082, V\-225083, V\-225084, V\-225086, V\-225087, V\-225088, V\-225089, V\-225092, V\-225093 and V\-236000
+ **Windows Server 2012 R2 MS STIG Version 3 Release 5**

  Includes all STIG settings that AWSTOE applies for Category III \(Low\) vulnerabilities, plus:

  V\-225574, V\-225573, V\-225572, V\-225571, V\-225570, V\-225569, V\-225568, V\-225567, V\-225566, V\-225565, V\-225564, V\-225563, V\-225562, V\-225561, V\-225560, V\-225559, V\-225558, V\-225557, V\-225555, V\-225554, V\-225553, V\-225551, V\-225550, V\-225549, V\-225548, V\-225546, V\-225545, V\-225544, V\-225543, V\-225542, V\-225541, V\-225540, V\-225539, V\-225538, V\-225535, V\-225534, V\-225533, V\-225532, V\-225531, V\-225530, V\-225529, V\-225528, V\-225527, V\-225524, V\-225523, V\-225522, V\-225521, V\-225520, V\-225519, V\-225518, V\-225517, V\-225516, V\-225515, V\-225513, V\-225510, V\-225509, V\-225508, V\-225506, V\-225504, V\-225503, V\-225502, V\-225501, V\-225500, V\-225494, V\-225486, V\-225478, V\-225477, V\-225475, V\-225474, V\-225472, V\-225471, V\-225470, V\-225469, V\-225464, V\-225463, V\-225461, V\-225458, V\-225457, V\-225456, V\-225455, V\-225454, V\-225453, V\-225452, V\-225448, V\-225443, V\-225442, V\-225441, V\-225415, V\-225414, V\-225413, V\-225411, V\-225410, V\-225409, V\-225408, V\-225407, V\-225406, V\-225405, V\-225404, V\-225402, V\-225401, V\-225400, V\-225398, V\-225397, V\-225395, V\-225393, V\-225391, V\-225389, V\-225386, V\-225385, V\-225384, V\-225383, V\-225382, V\-225381, V\-225380, V\-225379, V\-225378, V\-225377, V\-225375, V\-225374, V\-225373, V\-225372, V\-225371, V\-225370, V\-225369, V\-225368, V\-225367, V\-225356, V\-225353, V\-225352, V\-225351, V\-225350, V\-225349, V\-225348, V\-225347, V\-225346, V\-225345, V\-225344, V\-225341, V\-225340, V\-225339, V\-225338, V\-225337, V\-225329, V\-225326, V\-225325, V\-225317, V\-225316, V\-225315, V\-225314, V\-225305, V\-225304, V\-225303, V\-225302, V\-225301, V\-225300, V\-225299, V\-225298, V\-225297, V\-225296, V\-225295, V\-225294, V\-225293, V\-225292, V\-225291, V\-225290, V\-225289, V\-225288, V\-225287, V\-225286, V\-225285, V\-225284, V\-225283, V\-225282, V\-225281, V\-225280, V\-225279, V\-225278, V\-225277, V\-225276, V\-225275, V\-225273, V\-225272, V\-225271, V\-225270, V\-225269, V\-225268, V\-225267, V\-225266, V\-225265, V\-225264, V\-225263, V\-225261, V\-225260, V\-225259, and V\-225239
+ **Microsoft \.NET Framework 4\.0 STIG Version 2 Release 2**

  Includes all STIG settings that AWSTOE applies for Category III \(Low\) vulnerabilities, plus V\-225238
+ **Windows Firewall STIG Version 2 Release 1**

  Includes all STIG settings that AWSTOE applies for Category III \(Low\) vulnerabilities, plus:

  V\-241989, V\-241990, V\-241991, V\-241993, V\-241998, and V\-242003
+ **Internet Explorer 11 STIG Version 2 Release 3**

  Includes all STIG settings that AWSTOE applies for Category III \(Low\) vulnerabilities, plus:

  V\-46473, V\-46475, V\-46481, V\-46483, V\-46501, V\-46507, V\-46509, V\-46511, V\-46513, V\-46515, V\-46517, V\-46521, V\-46523, V\-46525, V\-46543, V\-46545, V\-46547, V\-46549, V\-46553, V\-46555, V\-46573, V\-46575, V\-46577, V\-46579, V\-46581, V\-46583, V\-46587, V\-46589, V\-46591, V\-46593, V\-46597, V\-46599, V\-46601, V\-46603, V\-46605, V\-46607, V\-46609, V\-46615, V\-46617, V\-46619, V\-46621, V\-46625, V\-46633, V\-46635, V\-46637, V\-46639, V\-46641, V\-46643, V\-46645, V\-46647, V\-46649, V\-46653, V\-46663, V\-46665, V\-46669, V\-46681, V\-46685, V\-46689, V\-46691, V\-46693, V\-46695, V\-46701, V\-46705, V\-46709, V\-46711, V\-46713, V\-46715, V\-46717, V\-46719, V\-46721, V\-46723, V\-46725, V\-46727, V\-46729, V\-46731, V\-46733, V\-46779, V\-46781, V\-46787, V\-46789, V\-46791, V\-46797, V\-46799, V\-46801, V\-46807, V\-46811, V\-46815, V\-46819, V\-46829, V\-46841, V\-46847, V\-46849, V\-46853, V\-46857, V\-46859, V\-46861, V\-46865, V\-46869, V\-46879, V\-46883, V\-46885, V\-46889, V\-46893, V\-46895, V\-46897, V\-46903, V\-46907, V\-46921, V\-46927, V\-46939, V\-46975, V\-46981, V\-46987, V\-46995, V\-46997, V\-46999, V\-47003, V\-47005, V\-47009, V\-64711, V\-64713, V\-64715, V\-64717, V\-64719, V\-64721, V\-64723, V\-64725, V\-64729, V\-72757, V\-72759, V\-72761, V\-72763, V\-75169, and V\-75171
+ **Microsoft Edge STIG Version 1 Release 6 \(Windows Server 2022 only\)**

  V\-235720, V\-235721, V\-235723, V\-235724, V\-235725, V\-235726, V\-235728, V\-235729, V\-235730, V\-235732, V\-235733, V\-235734, V\-235735, V\-235736, V\-235737, V\-235738, V\-235739, V\-235740, V\-235741, V\-235742, V\-235743, V\-235744, V\-235745, V\-235746, V\-235747, V\-235748, V\-235749, V\-235750, V\-235754, V\-235756, V\-235760, V\-235761, V\-235763, V\-235764, V\-235766, V\-235767, V\-235768, V\-235769, V\-235770, V\-235771, V\-235772, V\-235773, V\-235774, and V\-246736
+ **Defender STIG Version 2 Release 4 \(Windows Server 2022 only\)**

  V\-213427, V\-213429, V\-213430, V\-213431, V\-213432, V\-213433, V\-213434, V\-213435, V\-213436, V\-213437, V\-213438, V\-213439, V\-213440, V\-213441, V\-213442, V\-213443, V\-213444, V\-213445, V\-213446, V\-213447, V\-213448, V\-213449, V\-213450, V\-213451, V\-213455, V\-213464, V\-213465, and V\-213466

### STIG\-Build\-Windows\-High version 2022\.4\.0<a name="ib-windows-stig-high"></a>

The following list contains STIG settings that the component applies to your infrastructure\. If a setting isn't applicable for your infrastructure, the component skips that setting, and moves on\. For example, some STIG settings might not apply to standalone servers\. Organization\-specific policies can also affect which settings the component applies, such as a requirement for administrators to review document settings\. For more details about the STIGs that apply to Windows AMIs, you can download our [spreadsheet](https://aws-windows-downloads-us-west-1.s3.us-west-1.amazonaws.com/STIG/Win+STIG.xlsx)\.

For a complete list of Windows STIGs, see the [STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=windows)\. For information about how to view the complete list, see [STIG Viewing Tools](https://public.cyber.mil/stigs/srg-stig-tools/)\.

**Note**  
The STIG\-Build\-Windows\-High components include all STIG settings that AWSTOE applies for STIG\-Build\-Windows\-Low and STIG\-Build\-Windows\-Medium components, in addition to the STIG settings that apply specifically for Category I vulnerabilities\.
+ **Windows Server 2022 STIG Version 1 Release 1**

  V\-254293, V\-254352, V\-254353, V\-254354, V\-254374, V\-254378, V\-254381, V\-254446, V\-254465, V\-254466, V\-254467, V\-254469, V\-254474, V\-254475, and V\-254500
+ **Windows Server 2019 STIG Version 2 Release 5**

  Includes all STIG settings that AWSTOE applies for Categories II and III \(Medium and Low\) vulnerabilities, plus:

  V\-205653, V\-205654, V\-205711, V\-205713, V\-205724, V\-205725, V\-205757, V\-205802, V\-205804, V\-205805, V\-205806, V\-205849, V\-205908, V\-205913, V\-205914, and V\-205919
+ **Windows Server 2016 STIG Version 2 Release 5**

  Includes all STIG settings that AWSTOE applies for Categories II and III \(Medium and Low\) vulnerabilities, plus:

  V\-224874, V\-224932, V\-224933, V\-224934, V\-224954, V\-224958, V\-224961, V\-225025, V\-225044, V\-225045, V\-225046, V\-225048, V\-225053, V\-225054, and V\-225079
+ **Windows Server 2012 R2 MS STIG Version 3 Release 5**

  Includes all STIG settings that AWSTOE applies for Categories II and III \(Medium and Low\) vulnerabilities, plus:

  V\-225556, V\-225552, V\-225547, V\-225507, V\-225505, V\-225498, V\-225497, V\-225496, V\-225493, V\-225492, V\-225491, V\-225449, V\-225444, V\-225399, V\-225396, V\-225390, V\-225366, V\-225365, V\-225364, V\-225354, and V\-225274
+ **Microsoft \.NET Framework 4\.0 STIG Version 2 Release 2**

  Includes all STIG settings that AWSTOE applies for Categories II and III \(Medium and Low\) vulnerabilities for the Microsoft \.NET Framework\. No additional STIG settings apply for Category I vulnerabilities\.
+ **Windows Firewall STIG Version 2 Release 1**

  Includes all STIG settings that AWSTOE applies for Categories II and III \(Medium and Low\) vulnerabilities, plus:

  V\-241992, V\-241997, and V\-242002
+ **Internet Explorer 11 STIG Version 2 Release 3**

  Includes all STIG settings that AWSTOE applies for Categories II and III \(Medium and Low\) vulnerabilities for Internet Explorer 11\. No additional STIG settings apply for Category I vulnerabilities\.
+ **Microsoft Edge STIG Version 1 Release 6 \(Windows Server 2022 only\)**

  V\-235758 and V\-235759
+ **Defender STIG Version 2 Release 4 \(Windows Server 2022 only\)**

  V\-213426, V\-213452, and V\-213453

### STIG version history log for Windows<a name="ib-windows-version-hist"></a>

This section logs Windows component version history for the quarterly STIG updates\. To see the changes and published versions for a quarter, choose the title to expand the information\.

#### 2022 Q4 changes \- 02/01/2023:<a name="2022-q4-windows"></a>

Updated STIG versions and applied STIGS for the 2022 Q4 release as follows:

**STIG\-Build\-Windows\-Low version 2022\.4\.0**
+ Windows Server 2022 STIG Version 1 Release 1
+ Windows Server 2019 STIG Version 2 Release 5
+ Windows Server 2016 STIG Version 2 Release 5
+ Windows Server 2012 R2 MS STIG Version 3 Release 5
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 2
+ Windows Firewall STIG Version 2 Release 1
+ Internet Explorer 11 STIG Version 2 Release 3
+ Microsoft Edge STIG Version 1 Release 6 \(Windows Server 2022 only\)

**STIG\-Build\-Windows\-Medium version 2022\.4\.0**
+ Windows Server 2022 STIG Version 1 Release 1
+ Windows Server 2019 STIG Version 2 Release 5
+ Windows Server 2016 STIG Version 2 Release 5
+ Windows Server 2012 R2 MS STIG Version 3 Release 5
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 2
+ Windows Firewall STIG Version 2 Release 1
+ Internet Explorer 11 STIG Version 2 Release 3
+ Microsoft Edge STIG Version 1 Release 6 \(Windows Server 2022 only\)
+ Defender STIG Version 2 Release 4 \(Windows Server 2022 only\)

**STIG\-Build\-Windows\-High version 2022\.4\.0**
+ Windows Server 2022 STIG Version 1 Release 1
+ Windows Server 2019 STIG Version 2 Release 5
+ Windows Server 2016 STIG Version 2 Release 5
+ Windows Server 2012 R2 MS STIG Version 3 Release 5
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 2
+ Windows Firewall STIG Version 2 Release 1
+ Internet Explorer 11 STIG Version 2 Release 3
+ Microsoft Edge STIG Version 1 Release 6 \(Windows Server 2022 only\)
+ Defender STIG Version 2 Release 4 \(Windows Server 2022 only\)

#### 2022 Q3 changes \- 09/30/2022 \(no changes\):<a name="2022-q3-windows"></a>

There were no changes for Windows component STIGS for the 2022 third quarter release\.

#### 2022 Q2 changes \- 08/02/2022:<a name="2022-q2-windows"></a>

Updated STIG versions and applied STIGS for the 2022 Q2 release\.

**STIG\-Build\-Windows\-Low version 1\.5\.0**
+ Windows Server 2019 STIG Version 2 Release 4
+ Windows Server 2016 STIG Version 2 Release 4
+ Windows Server 2012 R2 MS STIG Version 3 Release 3
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 1
+ Windows Firewall STIG Version 2 Release 1
+ Internet Explorer 11 STIG Version 1 Release 19

**STIG\-Build\-Windows\-Medium version 1\.5\.0**
+ Windows Server 2019 STIG Version 2 Release 4
+ Windows Server 2016 STIG Version 2 Release 4
+ Windows Server 2012 R2 MS STIG Version 3 Release 3
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 1
+ Windows Firewall STIG Version 2 Release 1
+ Internet Explorer 11 STIG Version 1 Release 19

**STIG\-Build\-Windows\-High version 1\.5\.0**
+ Windows Server 2019 STIG Version 2 Release 4
+ Windows Server 2016 STIG Version 2 Release 4
+ Windows Server 2012 R2 MS STIG Version 3 Release 3
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 1
+ Windows Firewall STIG Version 2 Release 1
+ Internet Explorer 11 STIG Version 1 Release 19

#### 2022 Q1 changes \- 08/02/2022 \(no changes\):<a name="2022-q1-windows"></a>

There were no changes for Windows component STIGS for the 2022 first quarter release\.

#### 2021 Q4 changes \- 12/20/2021:<a name="2021-q4-windows"></a>

Updated STIG versions and applied STIGS for the 2021 fourth quarter release\.

**STIG\-Build\-Windows\-Low version 1\.5\.0**
+ Windows Server 2019 STIG Version 2 Release 3
+ Windows Server 2016 STIG Version 2 Release 3
+ Windows Server 2012 R2 MS STIG Version 3 Release 3
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 1
+ Windows Firewall STIG Version 2 Release 1
+ Internet Explorer 11 STIG Version 1 Release 19

**STIG\-Build\-Windows\-Medium version 1\.5\.0**
+ Windows Server 2019 STIG Version 2 Release 3
+ Windows Server 2016 STIG Version 2 Release 3
+ Windows Server 2012 R2 MS STIG Version 3 Release 3
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 1
+ Windows Firewall STIG Version 2 Release 1
+ Internet Explorer 11 STIG Version 1 Release 19

**STIG\-Build\-Windows\-High version 1\.5\.0**
+ Windows Server 2019 STIG Version 2 Release 3
+ Windows Server 2016 STIG Version 2 Release 3
+ Windows Server 2012 R2 MS STIG Version 3 Release 3
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 1
+ Windows Firewall STIG Version 2 Release 1
+ Internet Explorer 11 STIG Version 1 Release 19

#### 2021 Q3 changes \- 09/30/2021:<a name="2021-q3-windows"></a>

Updated STIG versions and applied STIGS for the 2021 third quarter release\.

**STIG\-Build\-Windows\-Low version 1\.4\.0**
+ Windows Server 2019 STIG Version 2 Release 2
+ Windows Server 2016 STIG Version 2 Release 2
+ Windows Server 2012 R2 MS STIG Version 3 Release 2
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 1
+ Windows Firewall STIG Version 1 Release 7
+ Internet Explorer 11 STIG Version 1 Release 19

**STIG\-Build\-Windows\-Medium version 1\.4\.0**
+ Windows Server 2019 STIG Version 2 Release 2
+ Windows Server 2016 STIG Version 2 Release 2
+ Windows Server 2012 R2 MS STIG Version 3 Release 2
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 1
+ Windows Firewall STIG Version 1 Release 7
+ Internet Explorer 11 STIG Version 1 Release 19

**STIG\-Build\-Windows\-High version 1\.4\.0**
+ Windows Server 2019 STIG Version 2 Release 2
+ Windows Server 2016 STIG Version 2 Release 2
+ Windows Server 2012 R2 MS STIG Version 3 Release 2
+ Microsoft \.NET Framework 4\.0 STIG Version 2 Release 1
+ Windows Firewall STIG Version 1 Release 7
+ Internet Explorer 11 STIG Version 1 Release 19

## Linux STIG components<a name="linux-os-stig"></a>

This section contains information about Linux STIG components, followed by a version history log\. If the Linux distribution doesn’t have STIG settings of its own, the component applies RHEL settings\. The component applies STIG settings to the infrastructure based on the Linux distribution, as follows:

**Red Hat Enterprise Linux \(RHEL\) 7 STIG settings**
+ RHEL 7
+ CentOS 7
+ Amazon Linux 2 \(AL2\)

**RHEL 8 STIG settings**
+ RHEL 8
+ CentOS 8

### STIG\-Build\-Linux\-Low version 2022\.4\.0<a name="ib-linux-stig-low"></a>

The following list contains STIG settings that the component applies to your infrastructure\. If a setting isn't applicable for your infrastructure, the component skips that setting, and moves on\. For example, some STIG settings might not apply to standalone servers\. Organization\-specific policies can also affect which settings the component applies, such as a requirement for administrators to review document settings\. For more details about the STIGs that apply to Linux AMIs, you can download our [spreadsheet](https://aws-windows-downloads-us-west-1.s3.us-west-1.amazonaws.com/STIG/Linux+STIG.xlsx)\.

For a complete list, see the [STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=operating-systems%2Cunix-linux)\. For information about how to view the complete list, see [STIG Viewing Tools](https://public.cyber.mil/stigs/srg-stig-tools/)\.

**RHEL 7 STIG Version 3 Release 9**
+ 

**RHEL 7/CentOS 7**  
V\-204452, V\-204576, and V\-204605
+ 

**AL2**  
V\-204452, V\-204576, and V\-204605

**RHEL 8 STIG Version 1 Release 8**
+ 

**RHEL 8/CentOS 8**  
V\-230241, V\-230253, V\-230269, V\-230270, V\-230281, V\-230285, V\-230346, V\-230381, V\-230395, V\-230468, V\-230469, V\-230485, V\-230486, V\-230491, V\-230494, V\-230495, V\-230496, V\-230497, V\-230498, V\-230499, and V\-244527

**Ubuntu 18\.04 STIG Version 2 Release 9**

V\-219163, V\-219164, V\-219165, V\-219172, V\-219173, V\-219174, V\-219175, V\-219178, V\-219180, V\-219210, V\-219301, V\-219327, V\-219332, and V\-219333

**Ubuntu 20\.04 STIG Version 1 Release 6**

V\-238202, V\-238221, V\-238222, V\-238223, V\-23822, V\-238226, V\-238234, V\-238235, V\-238237, V\-238308, V\-238323, V\-238357, V\-238362, and V\-238373

### STIG\-Build\-Linux\-Medium version 2022\.4\.0<a name="ib-linux-stig-medium"></a>

The following list contains STIG settings that the component applies to your infrastructure\. If a setting isn't applicable for your infrastructure, the component skips that setting, and moves on\. For example, some STIG settings might not apply to standalone servers\. Organization\-specific policies can also affect which settings the component applies, such as a requirement for administrators to review document settings\. For more details about the STIGs that apply to Linux AMIs, you can download our [spreadsheet](https://aws-windows-downloads-us-west-1.s3.us-west-1.amazonaws.com/STIG/Linux+STIG.xlsx)\.

For a complete list, see the [STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=operating-systems%2Cunix-linux)\. For information about how to view the complete list, see [STIG Viewing Tools](https://public.cyber.mil/stigs/srg-stig-tools/)\.

**Note**  
The STIG\-Build\-Linux\-Medium components include all STIG settings that AWSTOE applies for STIG\-Build\-Linux\-Low components, in addition to the STIG settings that apply specifically for Category II vulnerabilities\.

**RHEL 7 STIG Version 3 Release 9**

Includes all STIG settings that AWSTOE applies for Category III \(Low\) vulnerabilities, plus:
+ 

**RHEL 7/CentOS 7**  
V\-204405, V\-204406, V\-204407, V\-204408, V\-204409, V\-204410, V\-204411, V\-204412, V\-204413, V\-204414, V\-204415, V\-204416, V\-204417, V\-204418, V\-204422, V\-204423, V\-204426, V\-204427, V\-204428, V\-204431, V\-204435, V\-204437, V\-204449, V\-204450, V\-204451, V\-204457, V\-204466, V\-204503, V\-204516, V\-204517, V\-204521, V\-204524, V\-204531, V\-204536, V\-204537, V\-204538, V\-204539, V\-204540, V\-204541, V\-204542, V\-204543, V\-204544, V\-204545, V\-204546, V\-204547, V\-204548, V\-204549, V\-204550, V\-204551, V\-204552, V\-204553, V\-204554, V\-204555, V\-204556, V\-204557, V\-204558, V\-204559, V\-204560, V\-204562, V\-204563, V\-204564, V\-204565, V\-204566, V\-204567, V\-204568, V\-204572, V\-204579, V\-204584, V\-204585, V\-204586, V\-204587, V\-204589, V\-204590, V\-204591, V\-204592, V\-204593, V\-204598, V\-204599, V\-204600, V\-204601, V\-204602, V\-204609, V\-204610, V\-204611, V\-204612, V\-204613, V\-204614, V\-204615, V\-204616, V\-204617, V\-204619, V\-204622, V\-204624, V\-204625, V\-204630, V\-204631, V\-204633, V\-233307, V\-237634, V\-237635, and V\-251703
+ 

**AL2:**  
V\-204405, V\-204406, V\-204407, V\-204408, V\-204409, V\-204410, V\-204411, V\-204412, V\-204413, V\-204414, V\-204415, V\-204416, V\-204417, V\-204418, V\-204422, V\-204423, V\-204426, V\-204427, V\-204428, V\-204431, V\-204435, V\-204437, V\-204449, V\-204450, V\-204451, V\-204457, V\-204466, V\-204503, V\-204516, V\-204517, V\-204521, V\-204524, V\-204531, V\-204536, V\-204537, V\-204538, V\-204539, V\-204540, V\-204541, V\-204542, V\-204543, V\-204544, V\-204545, V\-204546, V\-204547, V\-204548, V\-204549, V\-204550, V\-204551, V\-204552, V\-204553, V\-204554, V\-204555, V\-204556, V\-204557, V\-204558, V\-204559, V\-204560, V\-204562, V\-204563, V\-204564, V\-204565, V\-204566, V\-204567, V\-204568, V\-204572, V\-204578, V\-204579, V\-204584, V\-204585, V\-204586, V\-204587, V\-204589, V\-204590, V\-204591, V\-204592, V\-204593, V\-204595, V\-204598, V\-204599, V\-204600, V\-204601, V\-204602, V\-204609, V\-204610, V\-204611, V\-204612, V\-204613, V\-204614, V\-204615, V\-204616, V\-204617, V\-204619, V\-204622, V\-204624, V\-204625, V\-204630, V\-204631, V\-204633, V\-233307, V\-237634, V\-237635, and V\-251703

**RHEL 8 STIG Version 1 Release 8**

Includes all STIG settings that AWSTOE applies for Category III \(Low\) vulnerabilities, plus:
+ 

**RHEL 8/CentOS 8**  
V\-230228, V\-230231, V\-230233, V\-230236, V\-230237, V\-230239, V\-230240, V\-230244, V\-230255, V\-230266, V\-230267, V\-230268, V\-230273, V\-230275, V\-230277, V\-230278, V\-230280, V\-230282, V\-230288, V\-230289, V\-230290, V\-230291, V\-230296, V\-230298, V\-230310, V\-230311, V\-230312, V\-230313, V\-230314, V\-230315, V\-230324, V\-230330, V\-230332, V\-230333, V\-230334, V\-230335, V\-230336, V\-230337, V\-230338, V\-230339, V\-230340, V\-230341, V\-230342, V\-230343, V\-230344, V\-230345, V\-230348, V\-230349, V\-230356, V\-230357, V\-230358, V\-230359, V\-230360, V\-230361, V\-230362, V\-230363, V\-230365, V\-230368, V\-230369, V\-230370, V\-230375, V\-230377, V\-230378, V\-230382, V\-230383, V\-230386, V\-230387, V\-230390, V\-230392, V\-230402, V\-230403, V\-230404, V\-230405, V\-230406, V\-230407, V\-230408, V\-230409, V\-230410, V\-230411, V\-230412, V\-230413, V\-230418, V\-230419, V\-230421, V\-230422, V\-230423, V\-230424, V\-230425, V\-230426, V\-230427, V\-230428, V\-230429, V\-230430, V\-230431, V\-230432, V\-230433, V\-230434, V\-230435, V\-230436, V\-230437, V\-230438, V\-230439, V\-230444, V\-230446, V\-230447, V\-230448, V\-230449, V\-230455, V\-230456, V\-230462, V\-230463, V\-230464, V\-230465, V\-230466, V\-230467, V\-230478, V\-230480, V\-230488, V\-230489, V\-230502, V\-230503, V\-230526, V\-230527, V\-230532, V\-230535, V\-230536, V\-230537, V\-230538, V\-230539, V\-230540, V\-230541, V\-230542, V\-230543, V\-230544, V\-230545, V\-230546, V\-230547, V\-230548, V\-230549, V\-230555, V\-230556, V\-230559, V\-230560, V\-230561, V\-237640, V\-237642, V\-237643, V\-244523, V\-244524, V\-244525, V\-244526, V\-244528, V\-244533, V\-244534, V\-244537, V\-244542, V\-244549, V\-244550, V\-244551, V\-244552, V\-244553, V\-244554, V\-250317, V\-251711, V\-251713, V\-251714, V\-251715, V\-251716, V\-251717, and V\-251718

**Ubuntu 18\.04 STIG Version 2 Release 9**

V\-219149, V\-219155, V\-219156, V\-219160, V\-219166, V\-219176, V\-219181, V\-219184, V\-219186, V\-219187, V\-219188, V\-219189, V\-219190, V\-219191, V\-219192, V\-219193, V\-219194, V\-219195, V\-219196, V\-219197, V\-219198, V\-219199, V\-219200, V\-219201, V\-219202, V\-219203, V\-219204, V\-219205, V\-219206, V\-219207, V\-219208, V\-219209, V\-219213, V\-219214, V\-219215, V\-219216, V\-219217, V\-219218, V\-219219, V\-219220, V\-219221, V\-219222, V\-219223, V\-219224, V\-219227, V\-219228, V\-219229, V\-219230, V\-219231, V\-219232, V\-219233, V\-219234, V\-219235, V\-219236, V\-219238, V\-219239, V\-219240, V\-219241, V\-219242, V\-219243, V\-219244, V\-219250, V\-219254, V\-219257, V\-219263, V\-219264, V\-219265, V\-219266, V\-219267, V\-219268, V\-219269, V\-219270, V\-219271, V\-219272, V\-219273, V\-219274, V\-219275, V\-219276, V\-219277, V\-219279, V\-219281, V\-219287, V\-219291, V\-219297, V\-219298, V\-219299, V\-219300, V\-219303, V\-219306, V\-219309, V\-219310, V\-219311, V\-219312, V\-219315, V\-219326, V\-219328, V\-219330, V\-219331, V\-219334, V\-219335, V\-219336, V\-219337, V\-219338, V\-219339, V\-219342, V\-233779, V\-233780, V\-237768, V\-237769, and V\-237770

**Ubuntu 20\.04 STIG Version 1 Release 6**

V\-238200, V\-238205, V\-238207, V\-238209, V\-238211, V\-238212, V\-238213, V\-238216, V\-238220, V\-238225, V\-238227, V\-238228, V\-238230, V\-238231, V\-238236, V\-238238, V\-238239, V\-238240, V\-238241, V\-238242, V\-238244, V\-238245, V\-238246, V\-238247, V\-238248, V\-238249, V\-238250, V\-238251, V\-238252, V\-238253, V\-238254, V\-238255, V\-238256, V\-238257, V\-238258, V\-238264, V\-238268, V\-238271, V\-238277, V\-238278, V\-238279, V\-238280, V\-238281, V\-238282, V\-238283, V\-238284, V\-238285, V\-238286, V\-238287, V\-238288, V\-238289, V\-238290, V\-238291, V\-238292, V\-238293, V\-238294, V\-238295, V\-238297, V\-238299, V\-238300, V\-238301, V\-238302, V\-238303, V\-238304, V\-238309, V\-238310, V\-238315, V\-238316, V\-238317, V\-238318, V\-238319, V\-238320, V\-238324, V\-238325, V\-238329, V\-238330, V\-238332, V\-238333, V\-238334, V\-238335, V\-238337, V\-238338, V\-238339, V\-238340, V\-238341, V\-238342, V\-238343, V\-238344, V\-238345, V\-238346, V\-238347, V\-238348, V\-238349, V\-238350, V\-238351, V\-238352, V\-238353, V\-238356, V\-238358, V\-238359, V\-238360, V\-238369, V\-238370, V\-238376, V\-238377, V\-238378, and V\-251505

### STIG\-Build\-Linux\-High version 2022\.4\.0<a name="ib-linux-stig-high"></a>

The following list contains STIG settings that the component applies to your infrastructure\. If a setting isn't applicable for your infrastructure, the component skips that setting, and moves on\. For example, some STIG settings might not apply to standalone servers\. Organization\-specific policies can also affect which settings the component applies, such as a requirement for administrators to review document settings\. For more details about the STIGs that apply to Linux AMIs, you can download our [spreadsheet](https://aws-windows-downloads-us-west-1.s3.us-west-1.amazonaws.com/STIG/Linux+STIG.xlsx)\.

For a complete list, see the [STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=operating-systems%2Cunix-linux)\. For information about how to view the complete list, see [STIG Viewing Tools](https://public.cyber.mil/stigs/srg-stig-tools/)\.

**Note**  
The STIG\-Build\-Linux\-High components include all STIG settings that AWSTOE applies for STIG\-Build\-Linux\-Low and STIG\-Build\-Linux\-Medium components, in addition to the STIG settings that apply specifically for Category I vulnerabilities\.

**RHEL 7 STIG Version 3 Release 9**

Includes all STIG settings that AWSTOE applies for Categories II and III \(Medium and Low\) vulnerabilities, plus:
+ 

**RHEL 7/CentOS 7**  
V\-204425, V\-204442, V\-204443, V\-204447, V\-204448, V\-204455, V\-204502, V\-204620, and V\-204621
+ 

**AL2:**  
V\-204425, V\-204442, V\-204443, V\-204447, V\-204448, V\-204455, V\-204502, V\-204620, and V\-204621

**RHEL 8 STIG Version 1 Release 8**

Includes all STIG settings that AWSTOE applies for Category III \(Low\) vulnerabilities, plus:
+ 

**RHEL 8/CentOS 8**  
V\-230264, V\-230265, V\-230487, V\-230492, V\-230529, V\-230531, V\-230533, and V\-230558

**Ubuntu 18\.04 STIG Version 2 Release 9**

V\-219157, V\-219158, V\-219177, V\-219212, V\-219308, V\-219313, V\-219314, V\-219316, V\-251506, and V\-251507

**Ubuntu 20\.04 STIG Version 1 Release 6**

V\-238201, V\-238215, V\-238218, V\-238219, V\-238326, V\-238327, V\-238380, V\-251503, and V\-251504

### STIG version history log for Linux<a name="ib-linux-version-hist"></a>

This section logs Linux component version history for the quarterly STIG updates\. To see the changes and published versions for a quarter, choose the title to expand the information\.

#### 2022 Q4 changes \- 02/01/2023:<a name="2022-q4-linux"></a>

Updated STIG versions and applied STIGS for the 2022 fourth quarter release as follows:

**STIG\-Build\-Linux\-Low version 2022\.4\.0**
+ RHEL 7 STIG Version 3 Release 9
+ RHEL 8 STIG Version 1 Release 8
+ Ubuntu 18\.04 STIG Version 2 Release 9
+ Ubuntu 20\.04 STIG Version 1 Release 6

**STIG\-Build\-Linux\-Medium version 2022\.4\.0**
+ RHEL 7 STIG Version 3 Release 9
+ RHEL 8 STIG Version 1 Release 8
+ Ubuntu 18\.04 STIG Version 2 Release 9
+ Ubuntu 20\.04 STIG Version 1 Release 6

**STIG\-Build\-Linux\-High version 2022\.4\.0**
+ RHEL 7 STIG Version 3 Release 9
+ RHEL 8 STIG Version 1 Release 8
+ Ubuntu 18\.04 STIG Version 2 Release 9
+ Ubuntu 20\.04 STIG Version 1 Release 6

#### 2022 Q3 changes \- 09/30/2022 \(no changes\):<a name="2022-q3-linux"></a>

There were no changes for Linux component STIGS for the 2022 third quarter release\.

#### 2022 Q2 changes \- 08/02/2022:<a name="2022-q2-linux"></a>

Introduced Ubuntu support, updated STIG versions and applied STIGS for the 2022 second quarter release as follows:

**STIG\-Build\-Linux\-Low version 2022\.2\.1**
+ RHEL 7 STIG Version 3 Release 7
+ RHEL 8 STIG Version 1 Release 6
+ Ubuntu 18\.04 STIG Version 2 Release 6 \(new\)
+ Ubuntu 20\.04 STIG Version 1 Release 4 \(new\)

**STIG\-Build\-Linux\-Medium version 2022\.2\.1**
+ RHEL 7 STIG Version 3 Release 7
+ RHEL 8 STIG Version 1 Release 6
+ Ubuntu 18\.04 STIG Version 2 Release 6 \(new\)
+ Ubuntu 20\.04 STIG Version 1 Release 4 \(new\)

**STIG\-Build\-Linux\-High version 2022\.2\.1**
+ RHEL 7 STIG Version 3 Release 7
+ RHEL 8 STIG Version 1 Release 6
+ Ubuntu 18\.04 STIG Version 2 Release 6 \(new\)
+ Ubuntu 20\.04 STIG Version 1 Release 4 \(new\)

#### 2022 Q1 changes \- 04/26/2022:<a name="2022-q1-linux"></a>

Refactored to include better support for containers\. Combined the previous AL2 script with RHEL 7\. Updated STIG versions and applied STIGS for the 2022 first quarter release as follows:

**STIG\-Build\-Linux\-Low version 3\.6**
+ RHEL 7 STIG Version 3 Release 6
+ RHEL 8 STIG Version 1 Release 5

**STIG\-Build\-Linux\-Medium version 3\.6**
+ RHEL 7 STIG Version 3 Release 6
+ RHEL 8 STIG Version 1 Release 5

**STIG\-Build\-Linux\-High version 3\.6**
+ RHEL 7 STIG Version 3 Release 6
+ RHEL 8 STIG Version 1 Release 5

#### 2021 Q4 changes \- 12/20/2021:<a name="2021-q4-linux"></a>

Updated STIG versions, and applied STIGS for the 2021 fourth quarter release as follows:

**STIG\-Build\-Linux\-Low version 3\.5\.1**
+ RHEL 7 STIG Version 3 Release 5
+ RHEL 8 STIG Version 1 Release 4

**STIG\-Build\-Linux\-Medium version 3\.5\.1**
+ RHEL 7 STIG Version 3 Release 5
+ RHEL 8 STIG Version 1 Release 4

**STIG\-Build\-Linux\-High version 3\.5\.1**
+ RHEL 7 STIG Version 3 Release 5
+ RHEL 8 STIG Version 1 Release 4

#### 2021 Q3 changes \- 09/30/2021:<a name="2021-q3-linux"></a>

Updated STIG versions, and applied STIGS for the 2021 third quarter release as follows:

**STIG\-Build\-Linux\-Low version 3\.4\.3**
+ RHEL 7 STIG Version 3 Release 4
+ RHEL 8 STIG Version 1 Release 3

**STIG\-Build\-Linux\-Medium version 3\.4\.3**
+ RHEL 7 STIG Version 3 Release 4
+ RHEL 8 STIG Version 1 Release 3

**STIG\-Build\-Linux\-High version 3\.4\.3**
+ RHEL 7 STIG Version 3 Release 4
+ RHEL 8 STIG Version 1 Release 3

## SCAP compliance validator component<a name="scap-compliance"></a>

The Security Content Automation Protocol \(SCAP\) is a set of standards that IT professionals can use to identify application security vulnerabilities for compliance\. The SCAP Compliance Checker \(SCC\) is a SCAP\-validated scanning tool, released by the Naval Information Warfare Center \(NIWC\) Atlantic\. For more information, see [Security Content Automation Protocol \(SCAP\) Compliance Checker \(SCC\)](https://www.niwcatlantic.navy.mil/scap/) on the *NIWC Atlantic* website\.

The AWSTOE `scap-checker-windows` and `scap-checker-linux` components download and install the SCC scanner on the pipeline build and test instances\. When the scanner runs, it performs authenticated configuration scans using DISA SCAP Benchmarks, and provides a report that includes the following information\. AWSTOE also writes the information to your application logs\.
+ STIG settings that are applied to the instance\.
+ An overall compliance score for the instance\.

We recommend that you run SCAP validation as the final step in your build process, to ensure that you report accurate compliance validation results\.

**Note**  
You can review the reports with one of the [STIG Viewing Tools](https://public.cyber.mil/stigs/srg-stig-tools/)\. These tools are available online via the DoD Cyber Exchange\.

The following sections describe the benchmarks that the SCAP validation components include\.

### scap\-checker\-windows version 2021\.04\.0<a name="scap-component-windows"></a>

The `scap-checker-windows` component runs on the Image Builder pipeline's build and test instances\. AWSTOE logs both the report and the score that the SCC application produces\.

The component performs the following workflow steps: 

1. Downloads and installs the SCC application\.

1. Imports the compliance benchmarks\.

1. Runs validation using the SCC application\.

1. Saves the compliance report and score locally on the build instance desktop\.

1. Logs the compliance score from the local report to the AWSTOE application log files\.

**Note**  
AWSTOE currently supports SCAP compliance validation for Windows Server 2012 R2, 2016, and 2019\.

The SCAP checker component for Windows includes the following benchmarks:

**SCC Version: 5\.4\.2**  
2021 Q4 Benchmarks:
+ U\_MS\_DotNet\_Framework\_4\-0\_V2R1\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_IE11\_V2R1\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_Windows\_2012\_and\_2012\_R2\_MS\_V3R2\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_Windows\_Defender\_AV\_V2R2\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_Windows\_Server\_2016\_V2R1\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_Windows\_Server\_2019\_V2R1\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_Windows\_Firewall\_V2R1\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_CAN\_Ubuntu\_18\-04\_V2R4\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_RHEL\_7\_V3R5\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_RHEL\_8\_V1R3\_STIG\_SCAP\_1\-2\_Benchmark

### scap\-checker\-linux version 2021\.04\.0<a name="scap-component-linux"></a>

The `scap-checker-linux` component runs on the Image Builder pipeline's build and test instances\. AWSTOE logs both the report and the score that the SCC application produces\.

The component performs the following workflow steps: 

1. Downloads and installs the SCC application\.

1. Imports the compliance benchmarks\.

1. Runs validation using the SCC application\.

1. Saves the compliance report and score locally, in the following location on the build instance: `/opt/scc/SCCResults`\.

1. Logs the compliance score from the local report to the AWSTOE application log files\.

**Note**  
AWSTOE currently supports SCAP compliance validation for RHEL 7/8 and Ubuntu 18\. The SCC application currently supports the x86 architecture for validation\.

The SCAP checker component for Linux includes the following benchmarks:

**SCC Version: 5\.4\.2**  
2021 Q4 Benchmarks:
+ U\_CAN\_Ubuntu\_18\-04\_V2R4\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_RHEL\_7\_V3R5\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_RHEL\_8\_V1R3\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_DotNet\_Framework\_4\-0\_V2R1\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_IE11\_V2R1\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_Windows\_2012\_and\_2012\_R2\_MS\_V3R2\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_Windows\_Defender\_AV\_V2R2\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_Windows\_Server\_2016\_V2R1\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_Windows\_Server\_2019\_V2R1\_STIG\_SCAP\_1\-2\_Benchmark
+ U\_MS\_Windows\_Firewall\_V2R1\_STIG\_SCAP\_1\-2\_Benchmark

### SCAP version history<a name="ib-scap-version-hist"></a>

The following table describes important changes to the SCAP environment and settings described in this document\.


| Change | Description | Date | 
| --- | --- | --- | 
|  Added SCAP components  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-stig.html)  | December 20, 2021 | 
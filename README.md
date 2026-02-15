<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>沖繩 2026 家族旅行行程</title>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700;900&display=swap');
        body { 
            font-family: 'Noto Sans TC', sans-serif; 
            -webkit-tap-highlight-color: transparent;
            overflow-x: hidden; /* 防止水平捲動 */
        }
        .animate-in { animation: fadeIn 0.5s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="bg-[#F0F4F8]">
    <div id="root"></div>

    <script type="text/babel">
        const { useState } = React;

        // --- 圖標組件 ---
        const Icon = ({ name, className = "w-5 h-5" }) => {
            const icons = {
                Home: <svg className={className} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V9Z"/><path d="M9 22V12h6v10"/></svg>,
                CheckList: <svg className={className} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"><path d="m9 11 3 3L22 4"/><path d="M21 12v7a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11"/></svg>,
                Lightbulb: <svg className={className} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"><path d="M15 14c.2-1 .7-1.7 1.5-2.5 1-.9 1.5-2.2 1.5-3.5A5 5 0 0 0 8 8c0 1.3.5 2.6 1.5 3.5.8.8 1.3 1.5 1.5 2.5"/><path d="M9 18h6"/><path d="M10 22h4"/></svg>,
                Languages: <svg className={className} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"><path d="m5 8 6 6"/><path d="m4 14 6-6 2-3"/><path d="M2 5h12"/><path d="M7 2h1"/><path d="m22 22-5-10-5 10"/><path d="M14 18h6"/></svg>,
                Plane: <svg className={className} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"><path d="M17.8 19.2 16 11l3.5-3.5C21 6 21.5 4 21 3c-1-.5-3 0-4.5 1.5L13 8 4.8 6.2c-.5-.1-1.1.1-1.5.5l-.3.3c-.4.4-.5 1-.3 1.5L9 12l-5 5H2l4 1 1 4v-2l5-5 3.5 6.3c.3.5.8.8 1.4.8l.3-.1c.4-.4.6-1 .5-1.5z"/></svg>,
                Phone: <svg className={className} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/></svg>,
                External: <svg className={className} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/></svg>,
                Clipboard: <svg className={className} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"><rect x="8" y="2" width="8" height="4" rx="1" ry="1"/><path d="M16 4h2a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h2"/></svg>,
                Hotel: <svg className={className} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"><path d="M18 12c.2 0 .4 0 .6.1l1.4-1.4c-.4-.4-1-.7-1.6-.7H5.6c-.6 0-1.2.3-1.6.7l1.4 1.4c.2-.1.4-.1.6-.1h12z"/><path d="M22 20V12c0-1.1-.9-2-2-2H4c-1.1 0-2 .9-2 2v8"/><path d="M7 14c.6 0 1 .4 1 1v5"/><path d="M17 14c.6 0 1 .4 1 1v5"/><path d="M2 18h20"/></svg>,
            };
            return icons[name] || icons.Plane;
        };

        const okinawaData = [
  { day: "D1", date: "2/20", theme: "抵達沖繩與北部啟航", activities: [{ time: "09:15", title: "抵達那霸機場 (OKA)", content: "辦理入境手續、提取行李。" }, { time: "10:30", title: "沖宮神社、波上宮", content: "沖繩總鎮守，佇立於懸崖上的絕美神社。" }, { time: "12:00", title: "午餐：泊港漁市場或漁師食堂", content: "享用新鮮海鮮或道地拉麵。" }, { time: "15:00", title: "許田休息站", content: "休息一下，買個伴手禮。(記得買美麗海的門票)" }, { time: "16:00", title: "Orion啤酒廠", content: "參觀裝瓶過程與品酒會。" }, { time: "17:00", title: "BIG EXPRESS 本部店", content: "購買晚餐物資。" }, { time: "18:00", title: "前往北部飯店休息", content: "欣賞高速公路沿線風景。" }] },
  { day: "D2", date: "2/21", theme: "北部自然與海洋饗宴", activities: [{ time: "10:30", title: "午餐：百年古家 大家 阿古豬", content: "在百年老宅內享用高品質阿古豬料理。" }, { time: "13:00", title: "古宇利島、古宇利大橋", content: "跨越絕美漸層藍海，參訪心型岩與海洋塔。" }, { time: "14:30", title: "今歸仁城", content: "古老城堡遺址，因其季節性盛開的櫻花而成為著名的取景地。。" }, { time: "16:30", title: "美麗海水族館", content: "世界級水族館，黑潮之海觀看巨型鯨鯊。(17:00有海豚表演)" }, { time: "18:30", title: "海人料理 海邦丸", content: "混凝土地板和木桌的極簡主義空間，供應烤魚、生魚片以及套餐。" }, ] },
  { day: "D3", date: "2/22", theme: "中部景觀與異國風情", activities: [{ time: "11:00", title: "萬座毛", content: "象鼻狀海蝕懸崖與遼闊草原。" }, { time: "13:00", title: "殘波岬燈塔", content: "壯麗海岸線與全白燈塔景致。" }, { time: "15:00", title: "AEON MALL/永旺STYLE 來客夢店", content: "逛街。" }, { time: "18:00", title: "Okinawa Green Lodge", content: "先check-in" }, { time: "19:00", title: "美國村", content: "美式風格的大型露天商場，有許多商店和餐廳進駐，並設有電影院與摩天輪。" }] },
  { day: "D4", date: "2/23", theme: "南部文化與奇景探索", activities: [{ time: "08:30", title: "普滿天宮", content: "這座古老神社的特色為小型鐘乳石洞穴，吸引遊客前來祈求好運。" },{ time: "10:00", title: "玉泉洞 / 王國村", content: "參觀日本第二大鐘乳石洞與琉球玻璃工藝。" }, { time: "12:00", title: "午餐：中本天婦羅店", content: "在地知名美味天婦羅，順便找貓咪玩耍。" }, { time: "14:00", title: "齋場御嶽", content: "綠意繁茂的聖地，琉球人相信這裡是女神阿摩美久現身之處。" }, { time: "16:00", title: "KOJIMA×BicCamera 那霸店", content: "逛街。" }, { time: "18:00", title: "國際通自由購物", content: "逛土產店、Don Quijote (驚安殿堂)。" }] },
  { day: "D5", date: "2/24", theme: "最後巡禮與回程", activities: [{ time: "09:00", title: "西來院", content: "達摩寺。" }, { time: "11:00", title: "琉球玻璃村", content: "手工藝中心，有彩繪與吹玻璃課程、互動式展覽及手工藝品攤商。" }, { time: "12:30", title: "沖繩 ASHIBINAA Outlet", content: "購物。" }, { time: "18:00", title: "抵達機場報到", content: "辦理登機，準備搭機返台。" }] }
];

const packingData = [
  { title: "證件與重要文件", items: ["護照正本  (至少 6 個月以上)", "台灣駕照正本", "日文譯本 (監理站申請)", "日圓現金 (多備零錢)", "信用卡 (實體卡片)"] },
  { title: "交通與支付用品", items: ["suica/PASMO/ICOCA  ", "手機設定Apple Pay/ipass money(可先儲值，內建有paypay可使用)"] },
  { title: "電子用品", items: ["日本網卡/eSIM (先確認開啟)", "行動電源 (需隨身登機)", "充電頭/線 (Type-C/Lightning)", "翻譯App/Google Map", "相機"] },
  { title: "衣物與隨身用品", items: ["防風外套 (沖繩海風大)",  "摺疊傘/雨衣", "環保購物袋 (日本減塑)"] },
  { title: "個人清潔與保養(日本偏乾燥)", items: ["牙刷、牙膏", "乳液", "護唇膏、護手霜", "口罩"] },
  { title: "醫藥與健康用品", items: ["OK繃", "常備藥品 (感冒、止瀉、止痛、暈車)", "濕紙巾", "袖珍包衛生紙(日本的衛生紙很貴)"] }
];


const etiquetteData = [
  { title: "⚠️ 自駕核心守則", content: ["右駕，但靠左行駛。遇到紅燈要左轉必須等綠燈。", "遇到「止まれ」標誌必須完全停下3秒，左右觀察後再走。", "禁止路邊違停，請找停車場。"] },
  { title: "首次赴日注意", content: ["垃圾分類非常嚴格，路上難找垃圾桶，建議隨身帶塑膠袋。", "電車或公眾場合請將手機轉靜音，不要大聲喧嘩。", "在神社時就算沒有祭拜也要脫帽已表示禮貌。日本禁止路邊抽菸，請找吸菸室或著有熄菸桶的地方"] },
  { title: "餐廳禮儀", content: ["點餐時若需服務，舉手輕聲說「すみません (Sumimasen)」即可。", "在商店街時禁止邊走邊吃，請在店家旁吃完。"] },
  { title: "日本退稅基本規則", content: ["通常消費滿 5,000 日圓即可免稅。", "接帳時出示護照跟店員說tax free（不是在機場退稅）", "一般物品（衣服、包包、電器）退稅後 可以馬上用、穿、背。", "消耗品（零食、藥妝、保養品）一定會被封袋，出境前不能拆、不能用"] }
];

const translationData = [
  { label: "你好", jp: "こんにちは。", romaji: "Konnichiwa." },
  { label: "沒問題/不用", jp: "大丈夫です。", romaji: "Daijoubu desu." },
  { label: "我不太會日文？", jp: "日本語があまり分かりません。？", romaji: "Chuushajou wa arimasu ka?" },
  { label: "請給我這個", jp: "これをください。", romaji: "Kore o kudasai." },
  { label: "不好意思 (叫人/借過)", jp: "すみません。", romaji: "Sumimasen." },
  { label: "謝謝", jp: "ありがとうございます。", romaji: "Arigatou gozaimasu." }
];
        const App = () => {
            const [activeTab, setActiveTab] = useState('home');

            const renderHome = () => (
                <div className="space-y-4 animate-in w-full max-w-md mx-auto pb-32">
                    <div className="bg-white rounded-[24px] p-5 shadow-sm border border-slate-50">
                        <div className="flex justify-between items-center mb-5 text-slate-400">
                            <span className="text-[10px] font-black uppercase tracking-widest">Flight Schedule</span>
                            <span className="text-[10px] font-black uppercase tracking-widest">2026 Winter</span>
                        </div>
                        <div className="space-y-6">
                            <div className="flex justify-between items-center px-1">
                                <div className="text-center">
                                    <div className="text-2xl font-black text-slate-800">06:55</div>
                                    <div className="text-[9px] font-bold text-slate-400 uppercase">TPE</div>
                                </div>
                                <Icon name="Plane" className="text-slate-100 w-8 h-8 rotate-90" />
                                <div className="text-center">
                                    <div className="text-2xl font-black text-slate-800">09:15</div>
                                    <div className="text-[9px] font-bold text-slate-400 uppercase">OKA</div>
                                </div>
                            </div>
                            <div className="border-t border-dashed border-slate-100"></div>
                            <div className="flex justify-between items-center px-1">
                                <div className="text-center">
                                    <div className="text-2xl font-black text-slate-800">20:20</div>
                                    <div className="text-[9px] font-bold text-slate-400 uppercase">OKA</div>
                                </div>
                                <Icon name="Plane" className="text-slate-100 w-8 h-8 -rotate-90" />
                                <div className="text-center">
                                    <div className="text-2xl font-black text-slate-800">21:00</div>
                                    <div className="text-[9px] font-bold text-slate-400 uppercase">TPE</div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <a href="https://www.vjw.digital.go.jp/" target="_blank" className="bg-[#EBF5FF] rounded-[24px] p-5 shadow-sm border border-blue-100 flex items-center justify-between active:scale-[0.98] transition-all">
                        <div className="flex items-center gap-3">
                            <div className="w-10 h-10 bg-white rounded-2xl flex items-center justify-center text-blue-500 shrink-0">
                                <Icon name="Clipboard" className="w-5 h-5" />
                            </div>
                            <div>
                                <h4 className="text-sm font-black text-slate-700">Visit Japan Web</h4>
                                <p className="text-[10px] text-slate-400 font-bold">入境與海關申報</p>
                            </div>
                        </div>
                        <Icon name="External" className="w-4 h-4 text-blue-400" />
                    </a>

                    <div className="bg-white rounded-[24px] p-5 shadow-sm border border-slate-50">
                        <div className="flex items-center gap-2 mb-4 text-blue-400 font-black text-sm"><Icon name="Hotel" /> 住宿飯店</div>
                        <div className="space-y-2">
                            <div className="p-3 bg-slate-50 rounded-xl">
                                <p className="text-[9px] font-black text-blue-400 uppercase">D1-D2 北部</p>
                                <h4 className="text-xs font-bold text-slate-700 break-words">Private villa Gushikumui</h4>
                            </div>
                            <div className="p-3 bg-slate-50 rounded-xl">
                                <p className="text-[9px] font-black text-blue-400 uppercase">D3-D4 那霸</p>
                                <h4 className="text-xs font-bold text-slate-700 break-words">Okinawa Green Lodge</h4>
                            </div>
                        </div>
                    </div>

                    <div className="bg-[#FF9B9B] rounded-[24px] p-5 text-white shadow-lg relative overflow-hidden">
                        <Icon name="Phone" className="absolute -right-4 -top-4 opacity-20 w-20 h-20" />
                        <h3 className="font-black text-sm mb-3">緊急聯繫</h3>
                        <div className="grid grid-cols-1 gap-2">
                            <div className="bg-white/20 p-3 rounded-xl backdrop-blur-sm">
                                <p className="text-[9px] font-black opacity-70">領務局專線</p>
                                <p className="text-xs font-black">+886-800-085-095</p>
                            </div>
                            <div className="bg-white/20 p-3 rounded-xl backdrop-blur-sm">
                                <p className="text-[9px] font-black opacity-70">駐那霸辦事處</p>
                                <p className="text-xs font-black">090-1942-1368</p>
                            </div>
                        </div>
                    </div>
                </div>
            );

            const renderList = () => (
                <div className="space-y-4 w-full max-w-md mx-auto pb-32 animate-in">
                    <div className="px-1 mb-4">
                        <h2 className="text-xl font-black text-slate-800">行李清單</h2>
                    </div>
                    {packingData.map((sec, i) => (
                        <div key={i} className="bg-white rounded-[24px] p-5 shadow-sm border border-slate-50">
                            <h3 className="text-sm font-black text-blue-400 mb-4 flex items-center gap-2">
                                <div className="w-1 h-3 bg-blue-400 rounded-full"></div>
                                {sec.title}
                            </h3>
                            <div className="space-y-3">
                                {sec.items.map((item, j) => (
                                    <label key={j} className="flex items-start gap-3 text-xs font-bold text-slate-600 cursor-pointer active:opacity-60">
                                        <input type="checkbox" className="w-5 h-5 rounded-md border-2 border-slate-200 accent-blue-500 shrink-0 mt-0.5" /> 
                                        <span className="leading-tight">{item}</span>
                                    </label>
                                ))}
                            </div>
                        </div>
                    ))}
                </div>
            );

            const renderInfo = () => (
                <div className="space-y-4 w-full max-w-md mx-auto pb-32 animate-in text-slate-800">
                    <div className="px-1 mb-4">
                        <h2 className="text-xl font-black text-slate-800">注意事項</h2>
                    </div>
                    {etiquetteData.map((item, i) => (
                        <div key={i} className="bg-white rounded-[24px] p-5 shadow-sm border border-slate-50 overflow-hidden">
                            <div className="flex items-center gap-2 mb-3">
                                <Icon name="Lightbulb" className="w-4 h-4 text-blue-500" />
                                <h3 className="text-base font-black text-slate-700">{item.title}</h3>
                            </div>
                            <ul className="space-y-1.5 bg-slate-50 p-4 rounded-xl list-disc list-inside">
                                {item.content.map((line, k) => (
                                    <li key={k} className="text-[11px] font-bold leading-relaxed text-slate-600 break-words">{line}</li>
                                ))}
                            </ul>
                        </div>
                    ))}
                </div>
            );

            const renderTranslate = () => (
                <div className="space-y-3 w-full max-w-md mx-auto pb-32 animate-in">
                    <div className="px-1 mb-4">
                        <h2 className="text-xl font-black text-slate-800">溝通小幫手</h2>
                    </div>
                    {translationData.map((p, i) => (
                        <div key={i} className="bg-white rounded-[24px] p-5 shadow-sm border border-slate-50 active:bg-blue-50 transition-colors">
                            <p className="text-[9px] font-bold text-slate-400 mb-1">{p.label}</p>
                            <p className="text-lg font-black text-slate-800">{p.jp}</p>
                            <p className="text-[10px] font-medium text-blue-400 italic">{p.romaji}</p>
                        </div>
                    ))}
                </div>
            );

            const renderDay = (idx) => (
                <div className="w-full max-w-md mx-auto pb-32 animate-in">
                    <div className="flex justify-between items-end mb-6 px-1">
                        <div>
                            <h2 className="text-3xl font-black text-slate-800 tracking-tight">{okinawaData[idx].date}</h2>
                            <p className="text-blue-500 font-black text-[10px] uppercase tracking-wider">{okinawaData[idx].theme}</p>
                        </div>
                        <div className="text-4xl font-black text-slate-100">{okinawaData[idx].day}</div>
                    </div>
                    <div className="space-y-4">
                        {okinawaData[idx].activities.map((act, i) => (
                            <div key={i} className="bg-white rounded-[24px] p-5 shadow-sm border border-slate-50 flex gap-4">
                                <div className="w-10 h-10 bg-slate-50 rounded-xl flex items-center justify-center text-slate-300 font-black text-[9px] shrink-0 mt-1">{act.time}</div>
                                <div className="flex-1 min-w-0">
                                    <h3 className="text-base font-black text-slate-700 mb-1 break-words">{act.title}</h3>
                                    <p className="text-[11px] text-slate-400 font-bold leading-relaxed break-words">{act.content}</p>
                                </div>
                            </div>
                        ))}
                    </div>
                </div>
            );

            return (
                <div className="min-h-screen bg-[#F0F4F8] flex flex-col w-full overflow-x-hidden">
                    <header className="w-full bg-[#9EB4CD] pt-12 pb-10 text-center shrink-0">
                        <p className="text-[9px] tracking-[0.5em] font-black text-white/80 uppercase">Family Trip 2026</p>
                        <h1 className="text-4xl font-black text-white tracking-tighter mt-1">OKINAWA</h1>
                    </header>

                    <main className="w-full px-4 py-6 flex-grow">
                        {activeTab === 'home' && renderHome()}
                        {activeTab === 'list' && renderList()}
                        {activeTab === 'info' && renderInfo()}
                        {activeTab === 'translate' && renderTranslate()}
                        {typeof activeTab === 'number' && renderDay(activeTab)}
                    </main>

                    {/* 底部導覽列 - 寬度優化 */}
                    <div className="fixed bottom-6 left-1/2 -translate-x-1/2 w-[94%] max-w-sm z-50">
                        <nav className="bg-white/90 backdrop-blur-xl border border-white/50 shadow-2xl rounded-[28px] p-1.5 flex items-center justify-between">
                            <div className="flex items-center gap-0.5">
                                <button onClick={() => setActiveTab('home')} className={`p-2.5 rounded-[18px] transition-all ${activeTab === 'home' ? 'bg-[#98B2D1] text-white' : 'text-slate-300'}`}>
                                    <Icon name="Home" className="w-5 h-5" />
                                </button>
                                <button onClick={() => setActiveTab('list')} className={`p-2.5 rounded-[18px] transition-all ${activeTab === 'list' ? 'bg-[#98B2D1] text-white' : 'text-slate-300'}`}>
                                    <Icon name="CheckList" className="w-5 h-5" />
                                </button>
                                <button onClick={() => setActiveTab('info')} className={`p-2.5 rounded-[18px] transition-all ${activeTab === 'info' ? 'bg-[#98B2D1] text-white' : 'text-slate-300'}`}>
                                    <Icon name="Lightbulb" className="w-5 h-5" />
                                </button>
                                <button onClick={() => setActiveTab('translate')} className={`p-2.5 rounded-[18px] transition-all ${activeTab === 'translate' ? 'bg-[#98B2D1] text-white' : 'text-slate-300'}`}>
                                    <Icon name="Languages" className="w-5 h-5" />
                                </button>
                            </div>
                            <div className="w-px h-6 bg-slate-100 mx-1"></div>
                            <div className="flex gap-0.5 pr-1">
                                {okinawaData.map((d, i) => (
                                    <button key={i} onClick={() => setActiveTab(i)} className={`w-8 h-8 rounded-full text-[9px] font-black transition-all ${activeTab === i ? 'text-blue-500 bg-blue-50' : 'text-slate-300'}`}>
                                        {d.day}
                                    </button>
                                ))}
                            </div>
                        </nav>
                    </div>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>

<!DOCTYPE html>
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
        body { font-family: 'Noto Sans TC', sans-serif; -webkit-tap-highlight-color: transparent; }
        .animate-in { animation: fadeIn 0.5s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        /* 隱藏捲軸但保留功能 */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="bg-[#F0F4F8]">
    <div id="root"></div>

    <script type="text/babel">
        const { useState } = React;

        // --- 圖標組件 (SVG 實作) ---
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
            { day: "D1", date: "2/20", theme: "抵達沖繩與北部啟航", activities: [{ time: "09:15", title: "抵達那霸機場 (OKA)", content: "辦理入境手續、提取行李。" }, { time: "10:30", title: "沖宮神社、波上宮", content: "沖繩總鎮守，佇立於懸崖上的絕美神社。" }, { time: "12:00", title: "午餐：泊港漁市場", content: "享用新鮮海鮮或道地拉麵。" }, { time: "15:00", title: "許田休息站", content: "休息一下，買個伴手禮。(記得買美麗海門票)" }, { time: "16:00", title: "Orion 啤酒廠", content: "參觀裝瓶過程與品酒會。" }, { time: "17:00", title: "BIG EXPRESS 本部店", content: "購買晚餐物資。" }, { time: "18:00", title: "前往飯店休息", content: "欣賞高速公路沿線風景。" }] },
            { day: "D2", date: "2/21", theme: "北部自然與海洋饗宴", activities: [{ time: "10:30", title: "百年古家 大家 阿古豬", content: "在百年老宅內享用高品質阿古豬料理。" }, { time: "13:00", title: "古宇利島、古宇利大橋", content: "跨越絕美漸層藍海，參訪心型岩與海洋塔。" }, { time: "14:30", title: "今歸仁城", content: "古老城堡遺址，著名的取景地。" }, { time: "16:30", title: "美麗海水族館", content: "觀看巨型鯨鯊。(17:00 有海豚表演)" }, { time: "18:30", title: "海人料理 海邦丸", content: "極簡風格空間，供應新鮮生魚片套餐。" }] },
            { day: "D3", date: "2/22", theme: "中部景觀與異國風情", activities: [{ time: "11:00", title: "萬座毛", content: "象鼻狀海蝕懸崖與遼闊草原。" }, { time: "13:00", title: "殘波岬燈塔", content: "壯麗海岸線與全白燈塔景致。" }, { time: "15:00", title: "永旺來客夢", content: "大型購物商場。" }, { time: "18:00", title: "Green Lodge", content: "辦理入住。" }, { time: "19:00", title: "美國村", content: "美式風格商場，適合散步與晚餐。" }] },
            { day: "D4", date: "2/23", theme: "南部文化與奇景探索", activities: [{ time: "08:30", title: "普天滿宮", content: "特色為小型鐘乳石洞穴。" },{ time: "10:00", title: "玉泉洞 / 王國村", content: "參觀日本第二大鐘乳石洞。" }, { time: "12:00", title: "午餐：中本天婦羅", content: "在地知名美味，順便找貓咪玩。" }, { time: "14:00", title: "齋場御嶽", content: "琉球聖地，自然與信仰的交匯。" }, { time: "16:00", title: "BicCamera 那霸", content: "電器與雜貨採買。" }, { time: "18:00", title: "國際通自由購物", content: "逛土產、驚安殿堂。" }] },
            { day: "D5", date: "2/24", theme: "最後巡禮與回程", activities: [{ time: "09:00", title: "西來院", content: "達摩寺。" }, { time: "11:00", title: "琉球玻璃村", content: "互動式展覽及手工藝品攤商。" }, { time: "12:30", title: "ASHIBINAA Outlet", content: "最後血拼。" }, { time: "18:00", title: "抵達機場報到", content: "辦理登機，準備返台。" }] }
        ];

        const packingData = [
            { title: "證件與重要文件", items: ["護照正本 (至少6個月效期)", "台灣駕照正本", "日文譯本 (監理站申請)", "日圓現金 (多備零錢)", "信用卡 (實體卡)"] },
            { title: "電子設備", items: ["網卡/eSIM (先確認開啟)", "行動電源 (隨身登機)", "充電器與線材", "Google Map 離線地圖"] },
            { title: "個人用品", items: ["防風外套 (海風大)", "摺疊傘/雨衣", "常備藥品 (止瀉、止痛)", "保濕乳液/護唇膏"] }
        ];

        const etiquetteData = [
            { title: "⚠️ 自駕守則", content: "左側通行。紅燈不能左轉。看到「止まれ」必須完全停下3秒。" },
            { title: "首次赴日注意", content: "垃圾難找，請隨身帶塑膠袋。公眾場合手機轉靜音，神社請脫帽表示禮貌。" },
            { title: "退稅基本規則", content: "滿 5,000 日圓可免稅。消耗品封袋後不可在日本拆封。" }
        ];

        const translationData = [
            { label: "你好", jp: "こんにちは。", romaji: "Konnichiwa." },
            { label: "不好意思 (叫人/借過)", jp: "すみません。", romaji: "Sumimasen." },
            { label: "請給我這個", jp: "これをください。", romaji: "Kore o kudasai." },
            { label: "謝謝", jp: "ありがとうございます。", romaji: "Arigatou gozaimasu." },
            { label: "沒問題", jp: "大丈夫です。", romaji: "Daijoubu desu." }
        ];

        const App = () => {
            const [activeTab, setActiveTab] = useState('home');

            const renderHome = () => (
                <div className="space-y-5 animate-in max-w-lg mx-auto pb-32">
                    {/* 航班卡片 */}
                    <div className="bg-white rounded-[24px] p-6 shadow-sm border border-slate-50">
                        <div className="flex justify-between items-center mb-6 text-slate-400">
                            <span className="text-[10px] font-black uppercase tracking-widest">BR112 / CI123</span>
                            <span className="text-[10px] font-black uppercase tracking-widest">2026 Winter</span>
                        </div>
                        <div className="space-y-6">
                            <div className="flex justify-between items-center px-2">
                                <div className="text-center">
                                    <div className="text-3xl font-black text-slate-800">06:55</div>
                                    <div className="text-[10px] font-bold text-slate-400 uppercase">Taipei</div>
                                </div>
                                <Icon name="Plane" className="text-slate-100 w-10 h-10 rotate-90" />
                                <div className="text-center">
                                    <div className="text-3xl font-black text-slate-800">09:15</div>
                                    <div className="text-[10px] font-bold text-slate-400 uppercase">Okinawa</div>
                                </div>
                            </div>
                            <div className="border-t border-dashed border-slate-100"></div>
                            <div className="flex justify-between items-center px-2">
                                <div className="text-center">
                                    <div className="text-3xl font-black text-slate-800">20:20</div>
                                    <div className="text-[10px] font-bold text-slate-400 uppercase">Okinawa</div>
                                </div>
                                <Icon name="Plane" className="text-slate-100 w-10 h-10 -rotate-90" />
                                <div className="text-center">
                                    <div className="text-3xl font-black text-slate-800">21:00</div>
                                    <div className="text-[10px] font-bold text-slate-400 uppercase">Taipei</div>
                                </div>
                            </div>
                        </div>
                    </div>

                    {/* VJW 連結 */}
                    <a href="https://www.vjw.digital.go.jp/" target="_blank" className="bg-[#EBF5FF] rounded-[24px] p-6 shadow-sm border border-blue-100 flex items-center justify-between active:scale-95 transition-transform">
                        <div className="flex items-center gap-4">
                            <div className="w-12 h-12 bg-white rounded-2xl flex items-center justify-center text-blue-500">
                                <Icon name="Clipboard" className="w-6 h-6" />
                            </div>
                            <div>
                                <h4 className="text-sm font-black text-slate-700">Visit Japan Web</h4>
                                <p className="text-[10px] text-slate-400 font-bold">入境、海關申報</p>
                            </div>
                        </div>
                        <Icon name="External" className="w-5 h-5 text-blue-400" />
                    </a>

                    {/* 住宿卡片 */}
                    <div className="bg-white rounded-[24px] p-6 shadow-sm border border-slate-50">
                        <div className="flex items-center gap-2 mb-4 text-blue-400 font-black text-sm"><Icon name="Hotel" /> 住宿飯店</div>
                        <div className="space-y-3">
                            <div className="p-4 bg-slate-50 rounded-2xl">
                                <p className="text-[9px] font-black text-blue-400 uppercase mb-0.5">D1-D2 北部</p>
                                <h4 className="text-sm font-black text-slate-700">Private villa Gushikumui</h4>
                            </div>
                            <div className="p-4 bg-slate-50 rounded-2xl">
                                <p className="text-[9px] font-black text-blue-400 uppercase mb-0.5">D3-D4 那霸</p>
                                <h4 className="text-sm font-black text-slate-700">Okinawa Green Lodge</h4>
                            </div>
                        </div>
                    </div>

                    {/* 緊急聯絡 */}
                    <div className="bg-[#FF9B9B] rounded-[24px] p-6 text-white shadow-lg relative overflow-hidden">
                        <Icon name="Phone" className="absolute -right-4 -top-4 opacity-20 w-24 h-24" />
                        <h3 className="font-black text-sm mb-4">緊急聯繫專線</h3>
                        <div className="space-y-2">
                            <div className="bg-white/20 p-3 rounded-xl backdrop-blur-sm">
                                <p className="text-[9px] font-black opacity-70">外交部領務局專線</p>
                                <p className="text-sm font-black">+886-800-085-095</p>
                            </div>
                            <div className="bg-white/20 p-3 rounded-xl backdrop-blur-sm">
                                <p className="text-[9px] font-black opacity-70">駐那霸辦事處</p>
                                <p className="text-sm font-black">090-1942-1368</p>
                            </div>
                        </div>
                    </div>
                </div>
            );

            const renderList = () => (
                <div className="space-y-4 max-w-lg mx-auto pb-32 animate-in">
                    <div className="px-2 mb-4">
                        <h2 className="text-2xl font-black text-slate-800">行李清單</h2>
                        <p className="text-sm text-slate-400 font-bold italic">準備妥當，安心出發</p>
                    </div>
                    {packingData.map((sec, i) => (
                        <div key={i} className="bg-white rounded-[24px] p-6 shadow-sm border border-slate-50">
                            <h3 className="text-sm font-black text-blue-400 mb-4 flex items-center gap-2">
                                <div className="w-1 h-4 bg-blue-400 rounded-full"></div>
                                {sec.title}
                            </h3>
                            <div className="space-y-4">
                                {sec.items.map((item, j) => (
                                    <label key={j} className="flex items-center gap-3 text-sm font-bold text-slate-600 cursor-pointer">
                                        <input type="checkbox" className="w-6 h-6 rounded-lg border-2 border-slate-200 accent-blue-500 shrink-0" /> 
                                        <span>{item}</span>
                                    </label>
                                ))}
                            </div>
                        </div>
                    ))}
                </div>
            );

            const renderInfo = () => (
                <div className="space-y-4 max-w-lg mx-auto pb-32 animate-in">
                    <div className="px-2 mb-4">
                        <h2 className="text-2xl font-black text-slate-800">注意事項</h2>
                        <p className="text-sm text-slate-400 font-bold italic">給自駕與日本新手家人</p>
                    </div>
                    {etiquetteData.map((item, i) => (
                        <div key={i} className="bg-white rounded-[24px] p-6 shadow-sm border border-slate-50">
                            <div className="flex items-center gap-2 mb-3">
                                <div className="w-8 h-8 bg-blue-50 rounded-lg flex items-center justify-center text-blue-500">
                                    <Icon name="Lightbulb" className="w-4 h-4" />
                                </div>
                                <h3 className="text-lg font-black text-slate-700">{item.title}</h3>
                            </div>
                            <p className="text-xs text-slate-500 font-bold leading-relaxed bg-slate-50 p-4 rounded-xl">{item.content}</p>
                        </div>
                    ))}
                </div>
            );

            const renderTranslate = () => (
                <div className="space-y-3 max-w-lg mx-auto pb-32 animate-in">
                    <div className="px-2 mb-4">
                        <h2 className="text-2xl font-black text-slate-800">溝通小幫手</h2>
                    </div>
                    {translationData.map((p, i) => (
                        <div key={i} className="bg-white rounded-[24px] p-5 shadow-sm border border-slate-50 active:bg-blue-50 transition-colors">
                            <p className="text-[10px] font-bold text-slate-400 mb-1">{p.label}</p>
                            <p className="text-xl font-black text-slate-800">{p.jp}</p>
                            <p className="text-[11px] font-medium text-blue-400 italic">{p.romaji}</p>
                        </div>
                    ))}
                </div>
            );

            const renderDay = (idx) => (
                <div className="max-w-lg mx-auto pb-32 animate-in">
                    <div className="flex justify-between items-end mb-8 px-2">
                        <div>
                            <h2 className="text-4xl font-black text-slate-800">{okinawaData[idx].date}</h2>
                            <p className="text-blue-500 font-black text-sm uppercase tracking-wider">{okinawaData[idx].theme}</p>
                        </div>
                        <div className="text-4xl font-black text-slate-100">{okinawaData[idx].day}</div>
                    </div>
                    <div className="space-y-4">
                        {okinawaData[idx].activities.map((act, i) => (
                            <div key={i} className="bg-white rounded-[24px] p-6 shadow-sm border border-slate-50 flex gap-4">
                                <div className="w-12 h-12 bg-slate-50 rounded-2xl flex items-center justify-center text-slate-300 font-black text-[10px] shrink-0">{act.time}</div>
                                <div>
                                    <h3 className="text-lg font-black text-slate-700">{act.title}</h3>
                                    <p className="text-xs text-slate-400 font-bold leading-relaxed">{act.content}</p>
                                </div>
                            </div>
                        ))}
                    </div>
                </div>
            );

            return (
                <div className="min-h-screen bg-[#F0F4F8] flex flex-col">
                    <header className="w-full bg-[#9EB4CD] pt-16 pb-12 text-center">
                        <p className="text-[10px] tracking-[0.6em] font-black text-white/80 uppercase">Family Trip 2026</p>
                        <h1 className="text-5xl font-black text-white tracking-tighter">Okinawa</h1>
                    </header>

                    <main className="w-full px-4 py-8 flex-grow">
                        {activeTab === 'home' && renderHome()}
                        {activeTab === 'list' && renderList()}
                        {activeTab === 'info' && renderInfo()}
                        {activeTab === 'translate' && renderTranslate()}
                        {typeof activeTab === 'number' && renderDay(activeTab)}
                    </main>

                    {/* 底部導覽列 */}
                    <div className="fixed bottom-6 left-1/2 -translate-x-1/2 w-[92%] max-w-md z-50">
                        <nav className="bg-white/90 backdrop-blur-xl border border-white shadow-2xl rounded-[32px] p-2 flex items-center justify-between">
                            <div className="flex items-center gap-1">
                                <button onClick={() => setActiveTab('home')} className={`p-3 rounded-[20px] transition-all ${activeTab === 'home' ? 'bg-[#98B2D1] text-white' : 'text-slate-400'}`}>
                                    <Icon name="Home" />
                                </button>
                                <button onClick={() => setActiveTab('list')} className={`p-3 rounded-[20px] transition-all ${activeTab === 'list' ? 'bg-[#98B2D1] text-white' : 'text-slate-400'}`}>
                                    <Icon name="CheckList" />
                                </button>
                                <button onClick={() => setActiveTab('info')} className={`p-3 rounded-[20px] transition-all ${activeTab === 'info' ? 'bg-[#98B2D1] text-white' : 'text-slate-400'}`}>
                                    <Icon name="Lightbulb" />
                                </button>
                                <button onClick={() => setActiveTab('translate')} className={`p-3 rounded-[20px] transition-all ${activeTab === 'translate' ? 'bg-[#98B2D1] text-white' : 'text-slate-400'}`}>
                                    <Icon name="Languages" />
                                </button>
                            </div>
                            <div className="w-px h-8 bg-slate-100"></div>
                            <div className="flex gap-1 pr-2">
                                {okinawaData.map((d, i) => (
                                    <button key={i} onClick={() => setActiveTab(i)} className={`w-9 h-9 rounded-full text-[10px] font-black transition-all ${activeTab === i ? 'text-blue-500 bg-blue-50' : 'text-slate-300'}`}>
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

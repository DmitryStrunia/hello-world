# hello-world
        public ActionResult IndexProtocols(int? page, bool _IsTekYear = true)
        {
            ViewBag._IsTekYear = _IsTekYear;
            //-----------
            string dtmin = _allbd.Protocol.Min(p => p.DataProt).Year.ToString();
            string dtmax = _allbd.Protocol.Max(p => p.DataProt).Year.ToString();
            int Idtmin = Int32.Parse(dtmin);
            int Idtmax = Int32.Parse(dtmax);
            List<string> lstyears = new List<string>();
            for (int i = Idtmin; i <= Idtmax; i++)
            {
                lstyears.Add(i.ToString());
            }
            SelectList PredYears = new SelectList(lstyears, dtmax);
            ViewBag.Years = PredYears;
            if (DateTime.Now.Year.ToString() == dtmax)
            {
                //ViewBag._IsTekYear = false;
                ViewBag.Years = PredYears;
                ViewBag.period = "за текущий год";
            }
            else
            {
                //ViewBag._IsTekYear = true;
                ViewBag.Years = PredYears;
                ViewBag.period = "Указать год";
            }
            SelectList _Tip = new SelectList(new string[] {
                "лазерная координатно-измерительная система Absolute Tracker AT901-MR",
                "координатно-измерительная машина Global Performance",
                "дальномер Leica DISTOtmD3aBT" });
            ViewBag.tip = _Tip;

            SelectList _TipProtocol = new SelectList(new string[] {
                "Стандартный",
                "С одним встроенным приложением",
                "С двумя встроенными приложениями",
                "Для лопасти ПСВ"});
            ViewBag.tipprotocol = _TipProtocol;


            //var perNSZ = _allbd.DepZak.Join(_allbd.Protocol, d => d.Zak_ID, p => p.Zak_ID,
            //(d, p) => new { d.NumSZ, p.Zak_ID }).Distinct().OrderByDescending(p => p.Zak_ID).ToList();
            var perNSZ = _allbd.DepZak.Distinct().OrderByDescending(p => p.Zak_ID).ToList();
            SelectList _prnumSZ = new SelectList(perNSZ, "NumSZ", "NumSZ");
            ViewBag.NSZ = _prnumSZ;


            int pageSize = 5;
            int pageNumber = (page ?? 1);
            var pr = _allbd.Protocol.OrderBy(i => i.Prot_ID);

            var per1 = _allbd.Protocol.Select(p => new { p.BazaName }).Distinct();
            SelectList _bn = new SelectList(per1, "BazaName", "BazaName");
            ViewBag.per1 = _bn;
            return View(pr.ToPagedList(pageNumber, pageSize));
        }

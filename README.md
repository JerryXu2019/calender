# calender
カレンダー
//当前月份第一天是周几，每个月有多少天，如何获取

ddkdhkdjbffhfkhlkhlsfljdnvjsnvs
 func getCountOfDaysInMonth(year: Int, month: Int) -> (count: Int, week: Int) {
        let dateFormatter = DateFormatter()
        dateFormatter.dateFormat = "yyyy-MM"
        let date = dateFormatter.date(from: String(year)+"-"+String(month))
        let calendar: Calendar = Calendar(identifier: .gregorian)
        
        let range = calendar.range(of: .day, in: .month, for: date!)
        let week = calendar.component(.weekday, from: date!)
        return ((range?.count)!, week)
    }
extension ViewController: UICollectionViewDelegate, UICollectionViewDataSource {
    // 返回Section的数量
    func numberOfSections(in collectionView: UICollectionView) -> Int {
        return 0
    }
    // 返回Item的数量
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
       return 0
    }
    // 返回Cell
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "dateItem", for: indexPath) as! dateCollectionViewCell
        return cell
    }  
}
override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        // 这两句话很重要！！！
        CalendarCollectionView.dataSource = self
        CalendarCollectionView.delegate = self
    }
    extension ViewController: UICollectionViewDelegate, UICollectionViewDataSource {
    // 返回Section的数量
    func numberOfSections(in collectionView: UICollectionView) -> Int {
        return 2
    }
    // 返回Item的数量
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        if section == 0 {
            return weekArray.count
        } else {
            return 42
        }
    }
    // 返回Cell
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "dateItem", for: indexPath) as! dateCollectionViewCell
        if indexPath.section == 0 {
            cell.textLabel.text = weekArray[indexPath.row]
        } else {
            var daysArray: [String] = []
            // 第一天之前的空白区域
            for number in 0..<firstDayOfMonth-1 {
                daysArray.append("")
            }
            for number in firstDayOfMonth-1...firstDayOfMonth+numberOfTheMonth-2 {
                daysArray.append(String(number-firstDayOfMonth+2))
            }
            // 最后一天后的空白区域
            for number in firstDayOfMonth+numberOfTheMonth-2...41 {
                daysArray.append("")
            }
            cell.textLabel.text = daysArray[indexPath.row]
        }
        return cell
    }
}

@IBAction func lastMonth(_ sender: UIButton) {
        if month == 1 {
            year -= 1
            month = 12
        }else {
            month -= 1
        }
        dateDisplayLabel.text = String(year)+"-"+String(month)
        firstDayOfMonth = date.getCountOfDaysInMonth(year: year, month: month).week
        numberOfTheMonth = date.getCountOfDaysInMonth(year: year, month: month).count
        CalendarCollectionView.reloadData()
    }

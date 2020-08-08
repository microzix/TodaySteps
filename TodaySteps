import SwiftUI
import HealthKit
import UIKit

struct ContentView2: View {
    var healthStore = HKHealthStore()
    @State var val = 0
    var body: some View {
        NavigationView{
        Form{
            Button(action: {
                getData()
            }) {
            Text("get the Health data")
            
            }
            Text("\(val)")
            
        }.navigationTitle("Health Informations")
        
        }
    }
    


    func getData() -> (Int) {
        authorizeHealthKit()
        return val
    }
    
    func authorizeHealthKit() {
        let share = Set([HKCategoryType.quantityType(forIdentifier: .stepCount)!])
        let read = Set([HKCategoryType.quantityType(forIdentifier: .stepCount)!])
        healthStore.requestAuthorization(toShare: share, read: read) { (chk, error) in
            if (chk){
                print("Permission Granted")
            }
            
        }
    }
    
    func todaySteps() {
        guard let sampleType = HKCategoryType.quantityType(forIdentifier: .stepCount) else {
            return
        }
        let startDate =  Calendar.current.startOfDay(for: Date())
        let predicate = HKQuery.predicateForSamples(withStart: startDate, end: Date(), options: .strictEndDate)
        var interval = DateComponents()
        interval.day = 1
        let query = HKStatisticsCollectionQuery(quantityType: sampleType, quantitySamplePredicate: predicate, options: [.cumulativeSum], anchorDate: startDate, intervalComponents: interval)
        query.initialResultsHandler = {
            query, result, error in
            if let myresult = result {
                myresult.enumerateStatistics(from: startDate, to: Date()) { (statistic, value) in
                    if let count = statistic.sumQuantity() {
                        let val = count.doubleValue(for: HKUnit.count())
                        print("today Steps: \(val)")
                    }
                    }
            }
        }
        healthStore.execute(query)
    }
    
}



struct ContentView2_Previews: PreviewProvider {
    static var previews: some View {
        ContentView2()
                
    }
}

---
title: Eureka LocationRow
image: assets/images/EurekaLocationRow.gif
categories:
- Tutorial
tags:
- featured
- eureka
- row
- location
- custom
- cell
- swift
- mapkit
- cllocation
---

If you're a newbie on this, you might not actually figure this out quick. Apparently, it's only a quick tour to the example project in Eureka. Particularly in [`CustomCells.swift`][location]. Just create a file and copy the last code below or from the link if the repository gets updated. 

```
<<< LocationRow(){
                $0.title = "Location"
                $0.value = CLLocation(latitude: latitude, longitude: longitude)
                $0.tag = "location"
                $0.validationOptions = .validatesOnChange //2
                $0.cellUpdate { (cell, row) in //3
                    if !row.isValid {
                        cell.textLabel?.textColor = .red
                    } else {
                        let lastLocation = row.value
                        let geocoder = CLGeocoder()
                        geocoder.reverseGeocodeLocation(lastLocation!,
                            completionHandler: { (placemarks, error) in
                                if error == nil {
                                    let place = placemarks![0]
                                    var adressString : String = ""
                                    if place.thoroughfare != nil {
                                        adressString = adressString + place.thoroughfare! + ", "
                                    }
                                    if place.subThoroughfare != nil {
                                        adressString = adressString + place.subThoroughfare! + " "
                                    }
                                    if place.locality != nil {
                                        adressString = adressString + place.locality! + " - "
                                    }
                                    if place.postalCode != nil {
                                        adressString = adressString + place.postalCode! + " "
                                    }
                                    if place.subAdministrativeArea != nil {
                                        adressString = adressString + place.subAdministrativeArea! + " - "
                                    }
                                    if place.country != nil {
                                        adressString = adressString + place.country!
                                    }
                                    
                                    self.address = adressString.trimmingCharacters(in: .whitespacesAndNewlines)
                                }
                        })
                    }
                }
            }
```

The code above is how I implemented LocationRow in my form. You'll see from here that I used a` Geocoder` in order to formulate an address string from the coordinates selected to a variable in the ViewController called `address`.  If you don't need this feature, just get rid of what's after `else`.

```
import Foundation
import UIKit
import MapKit
import Eureka

//MARK: LocationRow
public final class LocationRow: OptionsRow<PushSelectorCell<CLLocation>>, PresenterRowType, RowType {
    
    public typealias PresenterRow = MapViewController
    
    /// Defines how the view controller will be presented, pushed, etc.
    public var presentationMode: PresentationMode<PresenterRow>?
    
    /// Will be called before the presentation occurs.
    public var onPresentCallback: ((FormViewController, PresenterRow) -> Void)?
    
    public required init(tag: String?) {
        super.init(tag: tag)
        presentationMode = .show(controllerProvider: ControllerProvider.callback { return MapViewController(){ _ in } }, onDismiss: { vc in _ = vc.navigationController?.popViewController(animated: true) })
        
        displayValueFor = {
            guard let location = $0 else { return "" }
            let fmt = NumberFormatter()
            fmt.maximumFractionDigits = 4
            fmt.minimumFractionDigits = 4
            let latitude = fmt.string(from: NSNumber(value: location.coordinate.latitude))!
            let longitude = fmt.string(from: NSNumber(value: location.coordinate.longitude))!
            return  "\(latitude), \(longitude)"
        }
    }
    
    /**
     Extends `didSelect` method
     */
    public override func customDidSelect() {
        super.customDidSelect()
        guard let presentationMode = presentationMode, !isDisabled else { return }
        if let controller = presentationMode.makeController() {
            controller.row = self
            controller.title = selectorTitle ?? controller.title
            onPresentCallback?(cell.formViewController()!, controller)
            presentationMode.present(controller, row: self, presentingController: self.cell.formViewController()!)
        } else {
            presentationMode.present(nil, row: self, presentingController: self.cell.formViewController()!)
        }
    }
    
    /**
     Prepares the pushed row setting its title and completion callback.
     */
    public override func prepare(for segue: UIStoryboardSegue) {
        super.prepare(for: segue)
        guard let rowVC = segue.destination as? PresenterRow else { return }
        rowVC.title = selectorTitle ?? rowVC.title
        rowVC.onDismissCallback = presentationMode?.onDismissCallback ?? rowVC.onDismissCallback
        onPresentCallback?(cell.formViewController()!, rowVC)
        rowVC.row = self
    }
}

public class MapViewController : UIViewController, TypedRowControllerType, MKMapViewDelegate {
    
    public var row: RowOf<CLLocation>!
    public var onDismissCallback: ((UIViewController) -> ())?
    
    lazy var mapView : MKMapView = { [unowned self] in
        let v = MKMapView(frame: self.view.bounds)
        v.autoresizingMask = [.flexibleWidth, .flexibleHeight]
        return v
        }()
    
    lazy var pinView: UIImageView = { [unowned self] in
        let v = UIImageView(frame: CGRect(x: 0, y: 0, width: 50, height: 50))
        v.image = UIImage(named: "map_pin", in: Bundle(for: MapViewController.self), compatibleWith: nil)
        v.image = v.image?.withRenderingMode(.alwaysTemplate)
        v.tintColor = self.view.tintColor
        v.backgroundColor = .clear
        v.clipsToBounds = true
        v.contentMode = .scaleAspectFit
        v.isUserInteractionEnabled = false
        return v
        }()
    
    let width: CGFloat = 10.0
    let height: CGFloat = 5.0
    
    lazy var ellipse: UIBezierPath = { [unowned self] in
        let ellipse = UIBezierPath(ovalIn: CGRect(x: 0, y: 0, width: self.width, height: self.height))
        return ellipse
        }()
    
    
    lazy var ellipsisLayer: CAShapeLayer = { [unowned self] in
        let layer = CAShapeLayer()
        layer.bounds = CGRect(x: 0, y: 0, width: self.width, height: self.height)
        layer.path = self.ellipse.cgPath
        layer.fillColor = UIColor.gray.cgColor
        layer.fillRule = .nonZero
        layer.lineCap = .butt
        layer.lineDashPattern = nil
        layer.lineDashPhase = 0.0
        layer.lineJoin = .miter
        layer.lineWidth = 1.0
        layer.miterLimit = 10.0
        layer.strokeColor = UIColor.gray.cgColor
        return layer
        }()
    
    
    required public init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
    }
    
    public override init(nibName nibNameOrNil: String?, bundle nibBundleOrNil: Bundle?) {
        super.init(nibName: nil, bundle: nil)
    }
    
    convenience public init(_ callback: ((UIViewController) -> ())?){
        self.init(nibName: nil, bundle: nil)
        onDismissCallback = callback
    }
    
    public override func viewDidLoad() {
        super.viewDidLoad()
        view.addSubview(mapView)
        
        mapView.delegate = self
        mapView.addSubview(pinView)
        mapView.layer.insertSublayer(ellipsisLayer, below: pinView.layer)
        
        let button = UIBarButtonItem(barButtonSystemItem: .done, target: self, action: #selector(MapViewController.tappedDone(_:)))
        button.title = "Done"
        navigationItem.rightBarButtonItem = button
        
        if let value = row.value {
            let region = MKCoordinateRegion(center: value.coordinate, latitudinalMeters: 400, longitudinalMeters: 400)
            mapView.setRegion(region, animated: true)
        }
        else{
            mapView.showsUserLocation = true
        }
        updateTitle()
        
    }
    
    public override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        
        let center = mapView.convert(mapView.centerCoordinate, toPointTo: pinView)
        pinView.center = CGPoint(x: center.x, y: center.y - (pinView.bounds.height/2))
        ellipsisLayer.position = center
    }
    
    
    @objc func tappedDone(_ sender: UIBarButtonItem){
        let target = mapView.convert(ellipsisLayer.position, toCoordinateFrom: mapView)
        row.value = CLLocation(latitude: target.latitude, longitude: target.longitude)
        onDismissCallback?(self)
    }
    
    func updateTitle(){
        let fmt = NumberFormatter()
        fmt.maximumFractionDigits = 4
        fmt.minimumFractionDigits = 4
        let latitude = fmt.string(from: NSNumber(value: mapView.centerCoordinate.latitude))!
        let longitude = fmt.string(from: NSNumber(value: mapView.centerCoordinate.longitude))!
        title = "\(latitude), \(longitude)"
    }
    
    public func mapView(_ mapView: MKMapView, regionWillChangeAnimated animated: Bool) {
        ellipsisLayer.transform = CATransform3DMakeScale(0.5, 0.5, 1)
        UIView.animate(withDuration: 0.2, animations: { [weak self] in
            self?.pinView.center = CGPoint(x: self!.pinView.center.x, y: self!.pinView.center.y - 10)
        })
    }
    
    public func mapView(_ mapView: MKMapView, regionDidChangeAnimated animated: Bool) {
        ellipsisLayer.transform = CATransform3DIdentity
        UIView.animate(withDuration: 0.2, animations: { [weak self] in
            self?.pinView.center = CGPoint(x: self!.pinView.center.x, y: self!.pinView.center.y + 10)
        })
        updateTitle()
    }
}

```

[location]: https://github.com/xmartlabs/Eureka/blob/master/Example/Example/CustomCells.swift

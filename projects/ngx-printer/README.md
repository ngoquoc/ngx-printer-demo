A easy to use service to print the window, parts of a window (div) or an image.
Printing of Angular Templates or Components is possible.
You can print opening a new window / tab or using the current window.
A directive can also be used to mark and store an HTML-element.

See the DEMO App for examples.

# Usage
1. Add to main *app.module* imports
2. Use the *NgxPrinterService* where you like via DI (private printerService: NgxPrinterService)
3. Use the the functions provided by the service

## Options
### Open new window
The service prints by opening a new window.
There is an option *printWindowOpen* to change this behavoir.
Set printWindowOpen=false;

You can also set this option in .forRoot while importing the
module to the *app.module*

```javascript
    imports: [
    BrowserModule,
    NgxPrinterModule.forRoot({printOpenWindow: true})
  ],
```
### Rendering time
Before openening the print window the service to some time to render the print content.
Default time is 200ms.
You can adjust the time using the property *timeToWaitRender* (also in forRoot).

### Default CSS-Class
When printing to the current window the service creates an component with a
css-class called 'default'.
You can override this class name using the property *renderClass* (also in forRoot).
Make sure that the class is placed in your global styles (styles.css).


## Functions - How to print 
### Print current window
`this.printerService.printCurrentWindow();`

### Print div by id
`this.printerService.printDiv('idOfDivToPrint');`

### Print image src/url directly
`this.printerService.printImg('assets/bratwurst.jpg');`

### Print Angular TemplateRef or Component
```javascript
 @ViewChild('PrintTemplate')
  private PrintTemplateTpl: TemplateRef<any>;

printTemplate() {
    this.printerService.printAngular(this.PrintTemplateTpl);
  }
```
Beware: To print a component the component needs to be known by service (copy source and add it to entry
component of app.module).
Otherwise use printHTMLElement instead.

### Print HTML Element
```javascript
 @ViewChild(LittleDummyComponent, {read: ElementRef}) PrintComponent: ElementRef;

 printHTMLElementToCurrent() {
    this.printerService.printHTMLElement(this.PrintComponent.nativeElement);
  }
```

## Event of print window
If you want to check whether the print window is open or not subscribe to observable
*$printWindowOpen*

```javascript
this.printWindowSunscription = this.printerService.$printWindowOpen.subscribe(val => {
      console.log('Print window is open:', val);
});
```

## Directive ngxPrintItem
 There is an directive ngxPrintItem to mark and store an HTML-Element as an item which can be printed 
 later and anyhwere on the page.
 An id has to be set.
 These items are stored in the services observable *printerService.$printItems*.
 Use the async pipe to subscribe and the function `printPrintItem`  to print the item.

HTML:
```html
<span id="firstPrintItem" ngxPrintItem printName="First one" >A <b>first</b> span with an ngxPrintItem directive</span>

<div *ngFor="let prinItem of $printItems | async">
  <span>{{prinItem.id}} - {{prinItem.printName}}</span>
  <button (click)="printItem(prinItem)">Print me!</button>
</div>
```
TS:
```javascript
  printItem(itemToPrint: PrintItem) {
    this.printerService.printPrintItem(itemToPrint);
  }
});
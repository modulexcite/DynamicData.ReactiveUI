## Dynamic Data / ReactiveUI Adaptor

[![Join the chat at https://gitter.im/RolandPheasant/DynamicData.ReactiveUI](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/RolandPheasant/DynamicData.ReactiveUI?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

This is a very simple adaptor layer to assist with binding dynamic data observables with reactiveui�s ReactiveList object. Get it from https://www.nuget.org/packages/DynamicData.ReactiveUI

### What is dynamic data?

Dynamic data is rx for collections or more precisely observable changesets which handle adds, updates and removes. See  https://github.com/RolandPheasant/DynamicData for more details and source code.

### What are the benefits of the integration between Dynamic Data and ReactiveUI

Dynamic data has in the region of 50 collection specific operators and ReactiveUI rocks so why not get the best of both worlds. Look at the following code.

```csharp
var list = new ReactiveList<TradeProxy>();
var myoperation = somedynamicdatasource
					.Filter(trade=>trade.Status == TradeStatus.Live) 
					.Transform(trade => new TradeProxy(trade))
					.Sort(SortExpressionComparer<TradeProxy>.Descending(t => t.Timestamp))
					.ObserveOn(RxApp.MainThreadScheduler)
					.Bind(list) //This is the magic which updates the list with the observable
					.DisposeMany()
					.Subscribe()
```
As ```somedynamicdatasource``` changes the results are filtered by live trades, transformed into a proxy, put into a sort order and the reactive list will exactly reflect all this. When a tradeproxy is removed from the observable it is disposed and when  ```myoperation``` is disposed, all trade proxys are disposed.

That was easy, declarative and powerful.

Alternatively another root into the dynamic data sub system would be as follows:
```csharp
var list = new ReactiveList<TradeProxy>();
var myoperation = list.ToObservableChangeSet()
                     .Filter(trade=>trade.Status == TradeStatus.Live) 
                     // ... etc
```
Ok, I know ReactiveUI has DerivedReactiveList but dynamic data is DerivedReactiveList on steriods. Binding is just a small part of it.

I will as soon as I get the time document everything. In the meantime I have started putting together a wpf demo to illustate the usage and the capability of dynamic data https://github.com/RolandPheasant/TradingDemo







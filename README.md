# -
做游戏SDK，需要用到内购的知识。网上很多内购的代码、但是都不是很全面，使用之后都会有掉单或者其他苹果内购的bug没有处理。最近整理一下，自己一直在使用的内购方式

内购的流程就不写了。主要讲一下有坑的地方

1.支付之后，苹果的收据没有及时下发。或者应用问题导致没有收到苹果的收据（这个问题网上很多教程也有讲）
（1）针对这个问题：在没有确认服务器收到收据时，不要结束当前订单的内购监听
就是不要调用这句：[[SKPaymentQueue defaultQueue] finishTransaction:transaction];
（2）调用支付时：SKMutablePayment *payment = [SKMutablePayment paymentWithProduct:prodect];
        payment.applicationUsername = order_id;
        [[SKPaymentQueue defaultQueue] addPayment:payment];
  applicationUsername：这个字段是@property(nonatomic, copy, readwrite) NSString *applicationUsername NS_AVAILABLE_IOS(7_0);
可以在这个透传订单号，或者用户ID。对于透传什么，可以自己根据情况来决定。苹果坑爹的地方在于，有时候成功了也不会把这个字段透传过来，这部分

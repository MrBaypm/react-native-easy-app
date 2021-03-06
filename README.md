## react-native-easy-app （React Native One-stop solution）


[查看中文文档](README.zh-CN.md)

[Instruction Manual](https://www.jianshu.com/u/13623408c1aa)


### Installation

```bash 
npm install react-native-easy-app --save
``` 
or 

```bash
yarn add react-native-easy-app
```


### Features

  * Support for fast synchronous access to AsyncStorage
  * Support for flexible Http requests through optional configuration
  * Support for Flexible base widget (no sensory multi-screen adaptation)


### Quick Start 

   * Data Storage : XStorage
   
     * Implement a persistent data store manager
     
     ```jsx 
        export const RNStorage = {// RNStorage : Custom data store object
            token: undefined, // Custom string value
            isShow: undefined, // Custom bool value
            userInfo: undefined, // Custom object
        };
     ```
     
     ```jsx 
       import { XStorage } from 'react-native-easy-app';
        
       const initCallback = () => {
       
            // From now on, you can access the variables in RNStorage synchronously
            
            console.log(RNStorage.isShow); // equal to [ console.log(await AsyncStorage.getItem('isShow')) ]
            
            RNStorage.token = 'TOKEN1343DN23IDD3PJ2DBF3=='; // equal to [ await AsyncStorage.setItem('token',TOKEN1343DN23IDD3PJ2DBF3==') ]
            
            RNStorage.userInfo = {name: 'rufeng', age: 30}; // equal to [ await AsyncStorage.setItem('userInfo',JSON.stringify({ name:'rufeng', age:30})) ]
       };
       
       XStorage.initStorage(RNStorage, initCallback);   
               
     ```
    
   * Configurable Http request framework
   
     * All based on configuration (configuration optional, free to set)
     
      ```jsx 
      import { XHttpConfig } from 'react-native-easy-app';
      
      XHttpConfig().initHttpLogOn(true) // Print the Http request log or not
                    .initBaseUrl(ApiCredit.baseUrl) // BaseUrl
                    .initContentType(XHttpConst.CONTENT_TYPE_URLENCODED)
                    .initHeaderSetFunc((headers, request) => {
                       // Set the public header parameter here
                    })
                    .initParamSetFunc((params, request) => {
                     // Set the public params parameter here
                    })
                    .initParseDataFunc((result, request, callback) => {
                       let {success, json, response, message, status} = result;
                       // Specifies the specific data parsing method for the current app
                });
      ```
     
     * Send request template
     
     ```jsx 
        import { XHttp } from 'react-native-easy-app';
     
        let url = 'v1/account/login/';
        let param = {phone: '18600000000', authCode: '123456'};
        let header = {Authorization: "Basic Y3Rlcm1pbmF......HcVp0WGtI"};
        let callback = () => (success, json, message, status) => { // Request a result callback
             if (success) {
                showToast(JSON.stringify(json))
             } else {
                showToast(msg)
             }
        };
     
        * Settable parameters are spliced in builder form
        XHttp().url(url)
            .param(param)
            .header(header)
            .internal()
            .rawData()
            .pureText()
            .encodeURI()
            .timeout(10000)
            .extra({tag: 'xx'})
            .contentType('text/xml')
            .resendRequest(data, callback) // Rerequest (used to refresh accessToken to resend a request that has failed)
            .loadingFunc((loading)=> showLoading('Please wait for a moment ...', loading))
            .[formJson|formData|formEncoded]()
            .[get|post|put|patch|delete|options](callback);
       
     ```
     
     * request-send
     
      ```jsx
         import {XHttp} from 'react-native-easy-app';
         
         const url = 'https://www.google.com';
        
         * Synchronous request
         const response = await XHttp().url(url).execute('GET');
         const {success, json, message, status} = response;
         
         if(success){
            this.setState({content: JSON.stringify(json)})
         } else {
            showToast(message)
         }
         
         * Asynchronous requests
         XHttp().url(url).get((success, json, message, status)=>{
             if (success){
                this.setState({content: JSON.stringify(json)});
             } else {
                showToast(msg);
             }
         });
                 
         * Asynchronous requests
         XHttp().url(url).execute('GET')
         .then(({success, json, message, status}) => {
             if (success) {
                  this.setState({content: JSON.stringify(json)});
             } else {
                  showToast(message);
             }
          })
          .catch(({message}) => {
              showToast(message);
          })
        ```
     
     * Flexible base widget
     
     ```
        XImage
        XText
        XView
        XFlatList
        
        XImage Partial path online images depend on the Settings of image resource BaseUrl
        
        Can be configured as follows before use：
        
        XWidget
        .initResource(Assets)
        .initReferenceScreen(375, 677); // The component scales the reference screen size
     ```
    
 
  Please refer to the detailed usage method [example](https://github.com/chende008/react-native-easy-app-sample)


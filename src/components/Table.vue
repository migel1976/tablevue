<template>
    <div>
       <DxDataGrid
         id='grid'
         :show-borders='true'
         :data-source='ordersData'
         :repaint-changes-only='true'
        >
         <DxEditing
            :refresh-mode='refresh-mode'
            :allow-adding='true'
            :allow-updating='true'
            :allow-deleting='true'
            mode='cell'
         />
         <DxColumn
            data-field='CustomerID'
            caption='Customer'
         >
           <DxLookup
              :data-source='customersData'
              value-expr='Value'
              display-expr='Text'
           />
         </DxColumn>         
         <DxColumn
            data-field='OrderDate'
            data-type='date'
         />
         <DxColumn data-field='Freight' />
         <DxColumn data-field='ShipCountry' />
         <DxColumn 
                   data-field='ShipVia'
                   caption='Shipping Company'
                   data-type='number'
         >
            <DxLookup
                    :data-source='shippersData'
                    value-expr='Value'
                    display-expr='Text'
            />
        </DxColumn>
        <DxScrolling mode='virtual' />
      </DxDataGrid>
    </div>
</template>

<script>
    import {
        DxDataGrid,
        DxColumn,
        DxEditing,
        DxLookup,
        DxScrolling
    }
    from 'devextreme-vue/data-grid';
    import CustomStore from 'devextreme/data/custom_store';
    import ruMessages from 'devextreme/localization/messages/ru.json';
    import { locale, loadMessages, formatMessage } from 'devextreme/localization';

    /* import 'whatwg-fetch'; */
    const URL='https://js.devexpress.com/Demos/Mvc/api/DataGridWebApi';

    export default{
        mounted(){
            locale('ru');       
        },
        components:{
            DxDataGrid,
            DxColumn,
            DxEditing,
            DxLookup,
            DxScrolling
        },
        data(){
            return{
                ordersData:new CustomStore({
                    key:'OrderID',
                    load:()=>this.sendRequest(`${URL}/Orders`),
                    insert:(values)=>this.sendRequest(`{URL}/InsertOrder`,'POST',{
                        values:JSON.stringify(values)
                    }),
                    update:(key,values)=>this.sendRequest(`${URL}/UpdateOrder`,'PUT',{
                        key:key,
                        values:JSON.stringify(values)
                    }),
                    remove:(key)=>this.sendRequest(`${URL}/DeleteOrder`,'DELETE',{
                        key:key
                    })
                }),
                customersData:new CustomStore({
                    key:'Value',
                    loadMode:'raw',
                    load:()=>this.sendRequest(`${URL}/CustomersLookup`)
                }),
                shippersData:new CustomStore({
                    key:'Value',
                    loadMode:'raw',
                    load:()=>this.sendRequest(`${URL}/ShippersLookup`)
                }),
                request:[],
                refreshMode:'reshape'
            }
        },
        methods:{
            sendRequest(url, method, data){
            /* sendRequest(url, method){ */
                method=method || 'GET';
                data=data||{};
                const params=Object.keys(data).map((key)=>{
                    return `${encodeURIComponent(key)}=${encodeURIComponent(data[key])}`;
                }).join('&');
                if(method==='GET'){
                    return fetch(url,{
                        method:method,
                        credentials:'include'
                        }).then(result=>result.json().then(json=>{
                        if(result.ok) return json.data;
                        throw json.Message;
                        }));
               }//end if==='GET'
               return fetch(url,{
                    method:method,
                    body:params,
                    headers:{
                       'Content-Type':'application/x-www-form-urlencoded;charset=UTF-8'
                    },
                    credentials:'include'
                    }).then(result=>{
                    if(result.ok){
                        return result.text().then(text=>text && JSON.parse(text));
                    }else{
                        return result.json().then(json=>{
                            throw json.Message;
                      });
                    }
                });
            }//end sendRequest
        }//end methods
    }
</script>

<style scoped>
    #grid{
        height:440px;
    }
</style>
    
    
           
         

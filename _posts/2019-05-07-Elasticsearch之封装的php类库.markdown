---
layout: post
title:  "Elasticsearch之封装的php类库"
date:   2019-05-07 16:05:13 +0000
categories: xiaochang create
---

tp5使用elasticsearch的方法

1、进入tp5框架中，在commposer.json中加入

       "require": {
              "elasticsearch/elasticsearch": "~6.0"
       ,
      
 2、commposer更新
 
          commposer update
   
   成功会在vendor目录下多一个elasticsearch目录
      
      
3、引用类库
     
     引入elasticsearch类库
     
     use Elasticsearch\ClientBuilder;
     
     
     
     
4、封装的类库

        <?php

        namespace app\es\server;
        use Elasticsearch\ClientBuilder;

        class Es
        {

          CONST HOST = '127.0.0.1';
          CONST PORT = '9200';
            private static  $_instance = null;
            private static  $esClient = null;




            /**
             * 
             * @param [string] $index [索引]
             * @param [string] $type  [类型]
             * @param [int] $number_of_shards  [分片数量]
             * @param [int] $number_of_replicas  [副本数量]
             */
          private function __construct()
          {
            try{
              //链接elasticseach
              self::$esClient = ClientBuilder::create()->setHosts([self::HOST.':'.self::PORT])->build();

            }catch(\Exception $e){
              throw new \Exception('Elasticsearch 服务异常');
            }

            if(empty(self::$esClient)){
              throw new \Exception('Elasticsearch is error');
            }


          }


          //单例模式
          public static  function getInstance()
          {
            if(!(self::$_instance instanceof self)){
              self::$_instance = new self();
            }
            return self::$_instance;
          } 


          /*
          *获取所有索引下的文档
           */
          public function index(){
                return self::$esClient->search();
            }


          //创建索引
            //现在我们开始添加一个新的索引和一些自定义设置：
            /**
             * [创建索引]
             * @param  [string]  $index              [索引名称]
             * @param  [string]  $type               [类型名称]
             * @param  integer $number_of_shards   [分片数]
             * @param  integer $number_of_replicas [副本数]
             * @return [type]                      [description]
             */
            // public function create_index($index,$type,$number_of_shards = 1, $number_of_replicas = 0)
            // {
            //     $indexParams['index'] = $index;                         //索引名称  
            //     $indexParams['type'] = $type;                          //类型名称  
            //     $indexParams['body']['settings']['number_of_shards'] = 1;   //
            //     $indexParams['body']['settings']['number_of_replicas'] = 0; //副本0
            //     return self::$esClient->create($indexParams);
            // }

          /**
           * 新增操作
           * @param [string] $index [索引名称]
           * @param [string] $type  [类型名称]
           * @param [array] $data  []
           */
          public  function add($index,$type,$data)
          {
            if(empty($index) || empty($type) || !is_array($data) || empty($data)){
              return false;
            }
            $params = [
                'index'	 => $index,
                'type'   => $type,
                'body'	 => $data
              ];

            try{
              $result = self::$esClient->index($params);
            }catch(\Exception $e){
              throw new \Exception('Elasticsearch	服务异常');
            }
            return $result;
          }

          /**
           * [获取文档]
           * @param [string] $index [索引名称]
           * @param [string] $type  [类型名称]
           * @param  [array] $id    [通过id查询]
           * @return [type]        [description]
           */
          public function get($index,$type,$id)
          {
            if(empty($id) || empty($index) || empty($type)){
              return [];
            }
            $params = [
                'index'	 => $index,
                'type'   => $type,
                'id'	 => $id,
              ];

            try{
              $result = self::$esClient->get($params);
            }catch(\Exception $e){
              throw new \Exception('Elasticsearch	服务异常');
            }

            return $result;
          }

          /**
           * [search 查询]
           * @param  [string]  $index      [索引名称]
           * @param  [string]  $index_type [类型名称]
           * @param  [array]  $where      [查询条件]
           * @param  integer $from       [从第几条开始查]
           * @param  integer $size       [查几条]
           * @param  string  $type       [查询方法]
           * @return [type]              [description]
           */
          public function search($index, $index_type, $where, $from =0, $size = 10, $type = "match")
          {
            if(!is_array($where) || empty($where) || empty($index) || empty($index_type)){
              return [];
            }

            $params = [
               'index' => $index,
               'type'  => $index_type,
               'body'  => [
                  'query' => [
                      $type => $where,
                  ],
               ],
              'from' => $from,
                    'size' => $size
            ];

            try{
              $return = self::$esClient->search($params);
            }catch(\Exception $e){
              throw new \Exception('Elasticsearch	服务异常');
            }

            return  $return;
          }


          /**
           * 更新文档
           * @param  [string] $index  [索引名称]
           * @param  [string] $type   [类型名称]
           * @param  [string|int] $id     [id]
           * @param  array  $params [更新的数据]
           * @return [type]         [description]
           */
          public function update($index,$type,$id,$params = [])
          {
            if(empty($id) || empty($index) || empty($type)){
              return false;
            }
            if(!is_array($params) || empty($params)){
              return false;
            }

            $updateParams = [
              'index' => $index,
              'type'  => $type,
              'id'    => $id,
              'body'  => [
                'doc' => $params
              ],
            ];
            try{
              $result = self::$esClient->update($updateParams);
            }catch(\Exception $e){
              throw new \Exception('Elasticsearch	服务异常');
            }

            return $result;
          }

          /**
           * [删除文档]
           * @param  [string] $index  [索引名称]
           * @param  [string] $type   [类型名称]
           * @param  [string|int] $id     [id]
           * @return [type]        [description]
           */
          public function deleteDoc($index,$type,$id)
          {
            if(empty($id) || empty($index) || empty($type)){
              return false;
            }

            $params = [
                'index' => $index,
                'type'  => $type,
                'id'    => $id
            ];

            try{
              $result = self::$esClient->delete($params);
            }catch(\Exception $e){
              throw new \Exception('Elasticsearch	服务异常');
            }
            return $result;
          }

          /**
           * [deleteIndex description]
           * @param  [string] $index  [索引名称]
           * @return [type]        [description]
           */
          public function deleteIndex($index)
          {
            if(empty($index)){
              return false;
            }

            try{
              $result = self::$esClient->indices()->delete(['index' => $index]);
            }catch(\Exception $e){
              throw new \Exception('Elasticsearch	服务异常');
            }
            return $result;

          }


        }
        
        
5、使用
    $index = 'index'; //索引
		$type = 'type'; //类型
		$body = ['testField' => 'abc'];
 添加索引
 
        Es::getInstance()->add($index,$type,$body);
     
 分页查询

       Es::getInstance()->search($index,$type,$body);
    
 id获取文档
  
       Es::getInstance()->get($index,$type,$id);  
       
 更新
        
        $update['testField'] = 'acbds';
       Es::getInstance()->update($index,$type,$id,$update);
       
       
删除索引

      Es::getInstance()->deleteIndex($index);
      

删除文档

    Es::getInstance()->deleteDoc($index,$type,$id);
      
 
    
      


# layui_table_demo
layui表格模板 （运用于lotus后台）


html代码

```html
{extend name='public/base'}
{block name='content'}

<!-- Content Header (Page header) -->
<section class="content-header">
  <h1>
    {$item['item1']}
    <small>{$item['item2']}</small>
  </h1>
  <ol class="breadcrumb">
    <li><a href="#"><i class="fa fa-dashboard"></i> {$item['item1']}</a></li>
    <li class="active">{$item['item2']}</li>
  </ol>
</section>

<!-- Main content -->
<section class="content">
  <div class="panel panel-default">
    <div class="panel-heading">
      <h3 class="panel-title">搜索栏</h3>
    </div>

    <div class="box-body layui-form">
      <div class="layui-input-inline">
        <input type="text" name="idnum" id="idnum" placeholder="请输入编号" autocomplete="off" class="layui-input">
      </div>
    
      <div class="layui-input-inline">
        <input type="text" name="name" id="name" placeholder="请输入名称" autocomplete="off" class="layui-input">
      </div>

      <div class="layui-input-inline">
        <input type="text" name="partya" id="partya" placeholder="请输入甲方" autocomplete="off" class="layui-input">
      </div>

      <div class="layui-input-inline">
        <input type="text" name="partyb" id="partyb" placeholder="请输入乙方" autocomplete="off" class="layui-input">
      </div>

      <div class="layui-input-inline">
        <input type="text" name="signer" id="signer" placeholder="请输入签订人" autocomplete="off" class="layui-input">
      </div>

      <div class="layui-input-inline">
        <select name="contract_type_id" id="contract_type_id" lay-verify="required" lay-search="">
          <option value="">请选择项目名称</option>
          {volist name='ctlist' id='vc'}
            <option value="{$vc.id}">{$vc.name}</option>
          {/volist}
        </select>
      </div>

      <div class="layui-input-inline">
        <select name="department_id" id="department_id" lay-verify="required" lay-search="">
          <option value="">请选择所属部门</option>
          {volist name='dlist' id='vd'}
            <option value="{$vd.id}">{$vd.name}</option>
          {/volist}
        </select>
      </div>

      
      <div class="layui-input-inline">
        <input type="text" name="signingday" id="signingday" lay-verify="date" placeholder="请输入签约日期" autocomplete="off" class="layui-input">
      </div>

      <div class="layui-input-inline">
        <input type="text" name="addtime" id="addtime" lay-verify="date" placeholder="请输入创建日期" autocomplete="off" class="layui-input">
      </div>

      <button class="layui-btn layui-btn-normal layui-btn-sm" onclick="search()">搜索</button>
      <button class="layui-btn layui-btn-warm layui-btn-sm" onclick="javascript:location.reload()">刷新</button>
    </div>
  </div>

  <div class="row">
    <div class="col-xs-12">
      <!-- /.box -->
      <div class="box">
        <blockquote class="layui-elem-quote">
          <!-- <button class="layui-btn layui-btn-sm" onclick="new_contract()">新建合同</button> -->
          <button class="layui-btn layui-btn-sm layui-btn-normal" onclick="print_contract()">打印合同</button>
          <button class="layui-btn layui-btn-sm layui-btn-normal" onclick="excel_contract()">导出合同</button>
        </blockquote>

        <!-- /.box-header -->
        <div class="box-body">
          <!-- 表格 -->
          <table class="layui-hide" id="table_id" lay-filter="table_id"></table>
          <!-- 操作 -->
          <script type="text/html" id="action_bar">
            <a class="layui-btn layui-btn-xs" lay-event="details">查看详情</a>
            <!-- <a class="layui-btn layui-btn-normal layui-btn-xs" lay-event="append">追加合同</a> -->
            <!-- 超级管理员才能删除 -->
            <a class="layui-btn layui-btn-danger layui-btn-xs" lay-event="delete">删除</a>
          </script>

        </div>
        <!-- /.box-body -->
      </div>
      <!-- /.box -->
    </div>
    <!-- /.col -->
  </div>
  <!-- /.row -->

</section>
<!-- /.content -->
{/block}

<!-- 引入js 必须要这两个 2018年1月22日10:06:35-->
{block name='script'}
<script type="text/javascript">

  //初始化
  initTable("","","","","","","","","");

  //表格数据初始化渲染
  function initTable(name,idnum,partya,partyb,signer,contract_type_id,department_id,signingday,addtime) {
    layui.use('table', function () {
      var table = layui.table;
      //初始化表格
      table.render({
        elem: '#table_id',
        url: "{:url('admin/contract/contract_list')}",
        where: {
          name: name,
          idnum: idnum,
          partya: partya,
          partyb: partyb,
          signer: signer,
          contract_type_id:contract_type_id,
          department_id:department_id,
          signingday:signingday,
          addtime:addtime,
        }, //额外传递的参数
        page: { //支持传入 laypage 组件的所有参数（某些参数除外，如：jump/elem） - 详见文档
          limit:20,
          layout: [ 'prev', 'page', 'next'], //自定义分页布局
          groups: 3, //只显示 1 个连续页码
          first: 1, //不显示首页
          last:false //不显示尾页
        },
        cols: [
          [
            {type: 'checkbox', fixed: 'left'},
            {
              field: 'idnum',
              width: 150,
              title: '合同编号',
            },
            {
              field: 'name',
              width: 150,
              title: '合同名称',
              // edit: 'text'
            },
            {
              field: 'partya',
              minwidth: 100,
              title: '甲方',
            },
            {
              field: 'partyb',
              minwidth: 100,
              title: '乙方',
            },
            {
              field: 'signer',
              minwidth: 100,
              title: '签订人',
            },
            {
              field: 'signingday',
              minwidth: 100,
              title: '签约日期',
            },
            {
              field: 'amount',
              minwidth: 100,
              title: '合同金额',
            },
            {
              field: 'tname',
              minwidth: 100,
              title: '项目名称',
            },
            {
              field: 'adduser_name',
              minwidth: 100,
              title: '录入员',
            },
            {
              field: 'dname',
              minwidth: 100,
              title: '所属部门',
            },
            {
              field: 'addtime',
              minwidth: 100,
              title: '创建时间',
            },
            
            
            {
              fixed: 'right',
              title: '操作',
              toolbar: '#action_bar',
              width: 220
            }
          ]
        ],
        done: function(res, curr, count){
          //如果是异步请求数据方式，res即为你接口返回的信息。
          //如果是直接赋值的方式，res即为：{data: [], count: 99} data为当前页数据、count为数据总长度
          console.log(res.data);
          all_data = res.data;
          
        }
      });


      //表格工具栏
      table.on('tool(table_id)', function (obj) {
        var data = obj.data;
        console.log(data);
        if (obj.event === 'details') {
          //当事件为查看详情的时候,open一个页面,内容为data.content
          lotus_show('查看详情', 'details_contract.html?id=' + data.id, 0, 0);

        } else if(obj.event === 'append'){
          //追加合同跳转界面
          window.location.href="{:url('admin/contract/append_contract')}?id="+data.id;
        } 
         else if (obj.event === 'delete') {
          //当事件为修改的时候,跳转页面
          // window.location.href = "{:url('Article/update')}" + "?id=" + data.id;
          layer.confirm('真的确定删除吗？', function(index){
            //向服务端发送删除指令
            $.ajax({
                url: "{:url('admin/contract/delete_contract')}",
                type: 'get',
                data: {id: data.id},
            }).done(function(res){
                if(res.code==0){
                    layer.msg(res.msg,{icon:2});
                }else{
                    layer.msg(res.msg,{icon:1,time:3000},function(){
                      location.reload();  //刷新
                    });
                }
            })
          });
        }
      });

      ////////////// 复选框  //////////////////
      table.on('checkbox(table_id)', function(obj){
        console.log(obj.checked); //当前是否选中状态
        console.log(obj.data.id); //选中行的相关数据
        console.log(obj.type); //如果触发的是全选，则为：all，如果触发的是单选，则为：one
        if (obj.checked==true) {  //判断选中
          if (obj.type=='all') {  //判断全选
            printid = [];
            for (let ic = 0; ic < all_data.length; ic++) {
              printid.push(all_data[ic]['id'])
            }
          } else {  //判断单选
            printid.push(obj.data.id)
          }
        } else {  //判断取消
          if (obj.type=='all') {  //判断全选取消
            printid = [];
          } else {  //判断单选取消
            remove(obj.data.id);
          }
        }
        console.log("选中：",printid)
      });

    });
  }

  // ///选择打印初始化数据
  let all_data = [];
  let printid = [];
  //删除数组函数
  function indexOf(val) { 
    for (var i = 0; i < printid.length; i++) { 
      if (printid[i] == val) return i; 
    } 
    return -1; 
  };
  function remove(val) { 
    var index = indexOf(val); 
    if (index > -1) { 
      printid.splice(index, 1); 
    } 
  };

  /**
   * 搜索功能
   */
  function search() {
    var name = document.getElementById("name").value;
    var idnum = document.getElementById("idnum").value;
    var partya = document.getElementById("partya").value;
    var partyb = document.getElementById("partyb").value;
    var signer = document.getElementById("signer").value;

    var idxc=document.getElementById("contract_type_id").selectedIndex ; // selectedIndex代表的是你所选中项的index
    var contract_type_id = document.getElementById("contract_type_id").options[idxc].value;

    var idxd=document.getElementById("department_id").selectedIndex ; // selectedIndex代表的是你所选中项的index
    var department_id = document.getElementById("department_id").options[idxd].value;

    var signingday = document.getElementById("signingday").value;
    var addtime = document.getElementById("addtime").value;

    initTable(name,idnum,partya,partyb,signer,contract_type_id,department_id,signingday,addtime);
  }

  /**
  * 新增合同
  */
  function new_contract() {
    window.location.href="{:url('admin/contract/new_contract')}";
  }

  /**
  * excle导出合同
  */
  function excel_contract() {
    
    var name = document.getElementById("name").value;
    var idnum = document.getElementById("idnum").value;
    var partya = document.getElementById("partya").value;
    var partyb = document.getElementById("partyb").value;
    var signer = document.getElementById("signer").value;

    var idxc=document.getElementById("contract_type_id").selectedIndex ; // selectedIndex代表的是你所选中项的index
    var contract_type_id = document.getElementById("contract_type_id").options[idxc].value;

    var idxd=document.getElementById("department_id").selectedIndex ; // selectedIndex代表的是你所选中项的index
    var department_id = document.getElementById("department_id").options[idxd].value;

    var signingday = document.getElementById("signingday").value;
    var addtime = document.getElementById("addtime").value;

    window.location.href="{:url('admin/contract/excel_contract')}?name="+name+"&idnum="+idnum+"&partya="+partya+"&partyb="+partyb+"&signer="+signer+"&contract_type_id="+contract_type_id+"&department_id="+department_id+"&signingday="+signingday+"&addtime="+addtime+" ";
  }

  
  /**
   * @description: 打印合同
   * @param {type} 
   * @return: 
   */
  function print_contract() {
    if (printid.length == 0) {
      layer.msg("请勾选要打印的合同")
    } else {
      var all_id = JSON.stringify(printid);
      console.log(all_id)
      window.open("{:url('admin/contract/print_contract')}?all_id="+all_id); //新窗口
    }
  }
  
  
  /**日期插件**/
  layui.use(['form', 'laydate'], function(){
    var form = layui.form,
    laydate = layui.laydate;
    laydate.render({
      elem: '#signingday',
      range: '~'
    });
    laydate.render({
      elem: '#addtime',
      range: '~'
    });
  })

</script>
{/block}
```

控制器代码

```php
//先composer下载插件,再引用
// excel导出
use PhpOffice\PhpSpreadsheet\Spreadsheet;
use PhpOffice\PhpSpreadsheet\Writer\Xlsx;
use PhpOffice\PhpSpreadsheet\Style\Fill;
use PhpOffice\PhpSpreadsheet\Style\Color;
//pdf打印
use TCPDF;
use TCPDF_FONTS;


/**
* @msg: 合同列表界面
* @param {type}
* @return:
*/
public function list_contract()
{
    //项目名称列表
    $ctlist = Db::name('contract_type')->where('status', 1)->select();
    $this->assign('ctlist', $ctlist);
    //部门列表
    $dlist = Db::name('department')->where('status', 1)->select();
    $this->assign('dlist', $dlist);
    //标题传值
    $this->assign('item', ['item1'=>'合同管理','item2'=>'合同列表']);
    
    //当前登录账号的信息
    $this->assign('uid', session('uid'));
    $this->assign('department_id', session('department_id')); //所属部门id
    $this->assign('group_id', session('group_id')); //所属角色

    return $this->fetch();
}



/**
 * @msg: 合同列表
* @param {type} 
* @return: 
*/
public function contract_list()
{
    //获取分页page和limit参数
    $page=input("get.page")?input("get.page"):1;
    $page=intval($page);
    $limit=input("get.limit")?input("get.limit"):1;
    $limit=intval($limit);
    $start=$limit*($page-1);

    //条件
    $where=array();
    $idnum=input("get.idnum");
    if ($idnum!="") {
        $where['c.idnum']=array('like','%'.$idnum.'%');
    }
    $name=input("get.name");
    if ($name!="") {
        $where['c.name']=array('like','%'.$name.'%');
    }
    $partya=input("get.partya");
    if ($partya!="") {
        $where['c.partya']=array('like','%'.$partya.'%');
    }
    $partyb=input("get.partyb");
    if ($partyb!="") {
        $where['c.partyb']=array('like','%'.$partyb.'%');
    }

    $signer=input("get.signer");
    if ($signer!="") {
        $where['c.signer']=array('like','%'.$signer.'%');
    }

    $contract_type_id=input("get.contract_type_id");
    if ($contract_type_id!="") {
        $where['c.contract_type_id']=array('eq',$contract_type_id);
    }

    $department_id=input("get.department_id");
    if ($department_id!="") {
        $where['c.department_id']=array('eq',$department_id);
    }

    $signingday = input('get.signingday');
    if($signingday != ''){
        $tims= explode(" ~ ",$signingday);
        $stas = strtotime($tims[0]);
        $ends = strtotime($tims[1]);
        $where['c.signingday'] = array('between', array($stas,$ends));
    }

    $addtime = input('get.addtime');
    if($addtime != ''){
        $tim= explode(" ~ ",$addtime);
        $sta = strtotime($tim[0]);
        $end = strtotime($tim[1]);
        $where['c.addtime'] = array('between', array($sta,$end));
    }

    //根据角色和部门，显示合同信息
    $uid = session('uid');
    $department_id = session('department_id'); //所属部门id
    $group_id = session('group_id'); //所属角色

    if($group_id==251){	//假如角色是录入员
        $where['c.adduser'] = $uid; //录入人是自己
    }else if($group_id==250){ //如果是经理
        $where['c.department_id'] = $department_id; //录入部门是同部门
    }

    //分页查询
    $where['c.status'] = 1; //没被删除
    $res=Db::name("contract")
        ->alias('c')
        ->join('contract_type t','t.id = c.contract_type_id','left')
        ->join('department d','d.id = c.department_id','left')
        ->field('c.*,t.name as tname,d.name as dname')
        ->where($where)
        ->order('c.addtime desc, c.signingday desc')
        ->limit($start, $limit)
        ->select();
    
        
    
    //日期转换
    foreach ($res as $k => $v) {
        $res[$k]['signingday'] = date('Y-m-d',$v['signingday']);
        $res[$k]['addtime'] = date('Y-m-d H:i:s',$v['addtime']);
    }

    $list["msg"]="查询成功";
    $list["code"]=0;  //成功code是0
    $list["count"]=20;  //总数
    $list["data"]=$res;
    if (empty($res)) {
        $list["msg"]="暂无数据";
    }
    
    return json($list);
}



/**
 * @msg: 查看详情
* @param {type} 
* @return: 
*/
public function details_contract(){
    $id = input('param.id');
    $data = Db::name('contract')->where('id', $id)->find();
    $data['signingday'] = date('Y-m-d',$data['signingday']);
    $data['addtime'] = date('Y-m-d H:i:s',$data['addtime']);
    $data['contract_type_name'] = Db::name('contract_type')->where('id', $data['contract_type_id'])->value('name');
    $this->assign('data', $data);

    //查出最大的批次号
    $max_batch = Db::name('project')->where('contract_id', $id)->max('batch');
    $all_list = array(); 
    for ($i=0; $i < $max_batch; $i++) { 
        $plist = Db::name('project')->where('contract_id', $id)->where('batch',$i+1)->order('id asc')->select();
        $all_list[$i]['plist']=$plist;
    }		
    $this->assign('all_list', $all_list);

    //合同量 整合清单
    $pro_data=Db::name('project')
    ->where('contract_id', $id)
    ->field('*,sum(quantity) as sum_quantity,sum(total) as sum_total')
    ->group('number')
    ->order('id asc')
    ->select();


    $this->assign('pro_data', $pro_data);
    return $this->fetch();
}



/**
 * @msg: 删除合同
* @param {type} 
* @return: 
*/
public function delete_contract(){
    $id = input('param.id');
    $res = Db::name('contract')->where('id', $id)->update(['status'=>0]);
    if ($res) {
        $this->success('删除成功');
    } else {
        $this->error('删除失败');
    }
}



/**
 * @description: excel导出合同
* @param {type} 
* @return: 
*/
public function excel_contract(){
    //条件
    $where=array();
    $idnum=input("get.idnum");
    if ($idnum!="") {
        $where['c.idnum']=array('like','%'.$idnum.'%');
    }
    $name=input("get.name");
    if ($name!="") {
        $where['c.name']=array('like','%'.$name.'%');
    }
    $partya=input("get.partya");
    if ($partya!="") {
        $where['c.partya']=array('like','%'.$partya.'%');
    }
    $partyb=input("get.partyb");
    if ($partyb!="") {
        $where['c.partyb']=array('like','%'.$partyb.'%');
    }

    $signer=input("get.signer");
    if ($signer!="") {
        $where['c.signer']=array('like','%'.$signer.'%');
    }

    $contract_type_id=input("get.contract_type_id");
    if ($contract_type_id!="") {
        $where['c.contract_type_id']=array('eq',$contract_type_id);
    }

    $department_id=input("get.department_id");
    if ($department_id!="") {
        $where['c.department_id']=array('eq',$department_id);
    }

    $signingday = input('get.signingday');
    if($signingday != ''){
        $tims= explode(" ~ ",$signingday);
        $stas = strtotime($tims[0]);
        $ends = strtotime($tims[1]);
        $where['c.signingday'] = array('between', array($stas,$ends));
    }

    $addtime = input('get.addtime');
    if($addtime != ''){
        $tim= explode(" ~ ",$addtime);
        $sta = strtotime($tim[0]);
        $end = strtotime($tim[1]);
        $where['c.addtime'] = array('between', array($sta,$end));
    }

    //根据角色和部门，显示合同信息
    $uid = session('uid');
    $department_id = session('department_id'); //所属部门id
    $group_id = session('group_id'); //所属角色

    if($group_id==251){	//假如角色是录入员
        $where['c.adduser'] = $uid; //录入人是自己
    }else if($group_id==250){
        $where['c.department_id'] = $department_id; //录入部门是自己
    }

    //分页查询
    $where['c.status'] = 1; //没被删除
    $data=Db::name("contract")
        ->alias('c')
        ->join('contract_type t','t.id = c.contract_type_id','left')
        ->join('department d','d.id = c.department_id','left')
        ->field('c.*,t.name as tname,d.name as dname')
        ->where($where)
        ->order('c.addtime desc, c.signingday desc')
        ->select();
    
    // ***********excel导出*************//
    $spreadsheet = new Spreadsheet();
    $sheet = $spreadsheet->getActiveSheet();
    //设置当前工作表标题
    $spreadsheet->getActiveSheet()->setTitle('合同');
    // //设置字体颜色
    // $spreadsheet->getActiveSheet()->getStyle('A1:M1')->getFont()->getColor()->setARGB(\PhpOffice\PhpSpreadsheet\Style\Color::COLOR_RED);
    // //设置边框颜色
    // $worksheet = $spreadsheet->getActiveSheet();
    // $styleArray = [
    // 	'borders' => [
    // 		'outline' => [
    // 			'borderStyle' => \PhpOffice\PhpSpreadsheet\Style\Border::BORDER_THICK,
    // 			'color' => ['argb' => 'FFFF0000'],
    // 		],
    // 	],
    // ];
    // $worksheet->getStyle('A1:M1')->applyFromArray($styleArray);
    

    //设置背景颜色
    $spreadsheet->getActiveSheet()->getStyle('A1:M1')
                ->getFill()->setFillType(Fill::FILL_SOLID)
                ->getStartColor()->setARGB(Color::COLOR_YELLOW);

    $sheet->setCellValue('A1', '合同编号');
    $sheet->setCellValue('B1', '合同名称');
    $sheet->setCellValue('C1', '甲方');
    $sheet->setCellValue('D1', '乙方');
    $sheet->setCellValue('E1', '签订人');
    $sheet->setCellValue('F1', '签约日期');
    $sheet->setCellValue('G1', '合同金额');
    $sheet->setCellValue('H1', '大写金额');
    $sheet->setCellValue('I1', '项目名称');
    $sheet->setCellValue('J1', '录入员'); 
    $sheet->setCellValue('K1', '所属部门');
    $sheet->setCellValue('L1', '备注');
    $sheet->setCellValue('M1', '创建时间');

    $i=2;  //定义一个i变量，目的是在循环输出数据是控制行数
    $count = count($data);  //计算有多少条数据
    for ($i = 2; $i <= $count+1; $i++) {
        
        $sheet->setCellValue('A' . $i, $data[$i-2]['idnum']); 
        $sheet->setCellValue('B' . $i, $data[$i-2]['name']); 
        $sheet->setCellValue('C' . $i, $data[$i-2]['partya']); 
        $sheet->setCellValue('D' . $i, $data[$i-2]['partyb']); 
        $sheet->setCellValue('E' . $i, $data[$i-2]['signer']); 
        $sheet->setCellValue('F' . $i, date('Y-m-d',$data[$i-2]['signingday']) ); 
        $sheet->setCellValue('G' . $i, $data[$i-2]['amount']); 
        $sheet->setCellValue('H' . $i, $data[$i-2]['capital_amount']); 
        $sheet->setCellValue('I' . $i, $data[$i-2]['tname']); 
        $sheet->setCellValue('J' . $i, $data[$i-2]['adduser_name']); 
        $sheet->setCellValue('K' . $i, $data[$i-2]['dname']); 
        $sheet->setCellValue('L' . $i, $data[$i-2]['remark']); 
        $sheet->setCellValue('M' . $i, date('Y-m-d',$data[$i-2]['addtime']) ); 
        
    }
    
    header('Content-Type: application/vnd.ms-excel');
    header('Content-Disposition: attachment;filename="'.'合同'.date('Ymd',time()).'.xlsx"');
    header('Cache-Control: max-age=0');
    $writer = new Xlsx($spreadsheet);
    $writer->save('php://output');
    $spreadsheet->disconnectWorksheets();
    unset($spreadsheet);
    exit;
}



/**
 * @description: 打印合同
* @param {type} 
* @return: 
*/
public function print_contract(){
    //条件
    $all_id =	input('get.all_id');
    $all_id = json_decode($all_id);
    $where = array();
    $where['c.id'] = array('in',$all_id);

    $data=Db::name("contract")
        ->alias('c')
        ->join('contract_type t','t.id = c.contract_type_id','left')
        ->join('department d','d.id = c.department_id','left')
        ->field('c.*,t.name as tname,d.name as dname')
        ->where($where)
        ->order('c.addtime desc, c.signingday desc')
        ->select();

    
    /////////////////////// 打印控件//////////////////////

    
    // 调用打印 横向打印Landscape
    $pdf = new TCPDF('Landscape', PDF_UNIT, PDF_PAGE_FORMAT, true, 'UTF-8', false); 
    //参考网站：http://www.thinkphp.cn/code/2127.html 
    // 设置打印模式
    // $pdf->SetCreator(PDF_CREATOR);  
    // $pdf->SetAuthor('Nicola Asuni');  
    $pdf->SetTitle('打印预览');  
    // $pdf->SetSubject('TCPDF Tutorial');  
    // $pdf->SetKeywords('TCPDF, PDF, example, test, guide');  
    
        //删除预定义的打印 页眉/页尾
        $pdf->setPrintHeader(false);
        $pdf->setPrintFooter(false);
        // 设置页眉显示的内容 
    $pdf->SetHeaderData(PDF_HEADER_LOGO, PDF_HEADER_LOGO_WIDTH, PDF_HEADER_TITLE.' 038', PDF_HEADER_STRING);  
    // 设置页眉\页尾字体
    $pdf->setHeaderFont(Array(PDF_FONT_NAME_MAIN, '', PDF_FONT_SIZE_MAIN));  
    $pdf->setFooterFont(Array(PDF_FONT_NAME_DATA, '', PDF_FONT_SIZE_DATA));  
    // 设置默认等宽字体
    $pdf->SetDefaultMonospacedFont(PDF_FONT_MONOSPACED);  
    //设置左、上、右的间距 
    $pdf->SetMargins(PDF_MARGIN_LEFT, 0, PDF_MARGIN_RIGHT);  
    //页眉距离顶部的距离
    $pdf->SetHeaderMargin(PDF_MARGIN_HEADER); 
    //页尾距离底部的距离 
    $pdf->SetFooterMargin(PDF_MARGIN_FOOTER);  
    // 设置是否自动分页  距离底部多少距离时分页
    $pdf->SetAutoPageBreak(TRUE, PDF_MARGIN_BOTTOM);  
    // 设置图像比例因子
    $pdf->setImageScale(PDF_IMAGE_SCALE_RATIO);  

    // ---------------------------------------------------------  
    // 设置字体  
    $pdf->SetFont('stsongstdlight', '', 12,'', true);   
    
    foreach ($data as $kd => $vd) {
            // 增加页面
            $pdf->AddPage(); 
        
            $result = array();
            $result['idnum'] = $vd['idnum'];
            $result['name'] = $vd['name'];
            $result['tname'] = $vd['tname'];
            $result['partya'] = $vd['partya'];
            $result['partyb'] = $vd['partyb'];
            $result['signer'] = $vd['signer'];
            $result['signingday'] = date('Y-m-d',$vd['signingday']);
            $result['amount'] = $vd['amount'];
            $result['capital_amount'] = $vd['capital_amount'];
            $result['adduser_name'] = $vd['adduser_name'];
            $result['dname'] = $vd['dname'];
            $result['remark'] = $vd['remark'];
            $result['addtime'] = date('Y-m-d',$vd['addtime']);
            
            //合同量 整合清单
        $result['pro_list'] =Db::name('project')
        ->where('contract_id', $vd['id'])
        ->field('*,sum(quantity) as sum_quantity,sum(total) as sum_total')
        ->group('number')
        ->select();
            
            //这里放HTML代码 
            $tbl = $this->fetch('printpdf',$result);
            
            //输出html
            $pdf->writeHTML($tbl, true, false, false, false, '');  	
    }
    // ---------------------------------------------------------  
    //输出PDF文档 
    $pdf->Output(time().'.pdf', 'I'); 

}

```
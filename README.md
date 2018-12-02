## phalcon批量添加验证规则验证

###### 批量定义规则格式
```
[
	[验证字段1,验证类型[,错误提示,验证规则,验证条件(1:必须验证，0：存在则验证),其他参数],
	[验证字段2,验证类型[,错误提示,验证规则,验证条件,其他参数],
]
```

###### 使用
``` php
use PhalconValidate\Validate;

$data = [
	'username' => 'test',
	'age' => 10,
	'password' => '123456',
	'confirmPassword' => '123456',
	// ...
];

$rules = [
	[['username', 'age'], 'presenceof', ['username'=>'用户名不能为空','age'=>'年龄不能为空']],
	// 表示必须验证用户名且必须为数字字母
	['username', 'alnum', '用户名必须为数字字母', '', 1],
	['age', 'between', '年龄必须在0-150岁之间', [0,150]],
	['age', 'callback', '年龄必须小于200', function($data){return isset($data['age']) && $data['age']<200 ? true:false; },1],
	['password', 'confirmation', '两次密码必须一致', 'confirmPassword'],
	['start_time', 'date', '开始时间格式错误', 'Y-m-d H:i:s'],
	['status', 'inclusionin', '状态必须只能为0或1或2', [0,1,2], 1, ['strict' => true]],
	['tel', 'regex', '手机号格式错误', '/^1[34578]\d{9}$/', 1],
	// ...
];

$validate = new Validate();
$message = $validate->addRules($rules)->validate($data);
if (count($message)) {
	// 验证不通过
	var_dump($message);
}

```
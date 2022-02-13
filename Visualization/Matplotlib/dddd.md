```
$(function(){
    $('button').click(function(){
        $.ajax({
            url: 'starbucks/',
            dataType: 'json',
            success: function(msg){ ==> 성공하면 어떤 메세지 response해줌
                console.log(msg)
            }
        })
    })
})
```

```
return JsonResponse(result_dict) 딕셔너리형태의값을 넣어주면 자동으로 json객체로 만들어줌
```

변수 호이스팅
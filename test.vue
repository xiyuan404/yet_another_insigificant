<script>
router.post('/page', async(ctx, next)=>{
	const args = [
  	{ field: 'page', type: 'number'},
  	{ field: 'count', type: 'number'}
	]
	const requestBody = ctx.request.body // { page: 1, count: 10 }
	args.forEach(arg => {
  	if(!Object.keys(requestBody).includes(arg.field))	{
  		ctx.body = {
  			errcode: -1,
  			errmsg: '参数个数错误'
  		}
  		return; 
  	}else {
  		if(typeof requestBody[arg.field] != arg.type){
  			ctx.body = {
  				errcode: -2,
  				errmsg: '参数类型错误'
  			}
  			return;
  		}
  		
  		
  	}

    ctx.body = {
      errcode: 0,
      errmsg: 'ok',
      result: null
    }
	})  
})


</script>


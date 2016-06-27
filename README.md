var scode="";
var tokens;
var buffer="";

function isOp(c){
	if(c==' ' || c=='\n' || c=='\r' || c=='\t'|| c=='+' || c=='-' || c=='{' || c=='}' || c=='(' || c==')' ||c=='*' || c=='/' || c=='=' || c=='>' || c=='<' ||c=='.' || c==','|| c=='"'){
		return true;
	}
	else return false;
}

function scanner(){
	var i=0;
	var j[21000000000000];
	var k=0;
	tokens=new Array();
  	var c;
	var state=0; //0이면 연산자 가리기 모드
				 //1이면 단어 만들기 모드
            //2이면 숫자 만들기 모드
            //3이면 주석
            //4이면 문자열
	for(; i<scode.length; i++){
		c=scode.charAt(i);
		if(state==0){		
        if(c==' ' || c=='\n' || c=='\r' || c=='\t') continue;
			if(c=='"'){
				buffer='"';
				state=4;
				continue;
			}
        var nc=(i<scode.length-1)?scode.charAt(i+1):'@';
			if(c=='/'&&c=='/'){
				i++;
				state=3;
				continue;
			}
      if(j[k]==숫자&&'='&&('+'||'-'||'*'||'/')){
      i++;
      state=5;
      continue;
			if(c=='-' || c=='+' || c=='*' || c=='='){
				if(nc==c){
					tokens.push(c+c); // -- , ++ , ** , ==
					i++;
				}else if(nc=='='){
					tokens.push(c+'='); // -= , += , *= 
					i++;
				}else{
					tokens.push(c); // - , + , * , =
				}
				continue;
			}else if(c=='>' || c=='<' || c=='/'){
				if(nc=='='){
					tokens.push(c+'='); // >= , <= , /=
					i++;
				}else{
					tokens.push(c); // > , < , /
				}
				continue;
			}else{
            	switch(c){
                	case '{': tokens.push(c); break; // {
                	case '}': tokens.push(c); break; // }
                	case '(': tokens.push(c); break; // (
						case ')': tokens.push(c); break; // )
						case '.': tokens.push(c); break; // .
						case ',': tokens.push(c); break; // ,
                	default:
                	    if(isNaN(c)){
                	        state=1; buffer=c; // 연산자도 아니고 숫자도 아닌것이 시작됐다. 식별자(변수명, 함수명, 예약어 등 단어)의 시작
                    }else{
                        	state=2; buffer=c; // 숫자 추리기 시작
                    }
							break;
        		 }
			}
		}else if(state==1){
		    if(isOp(c)){
				i--;
				state=0;
				tokens.push(buffer);
				buffer="";
			}else{
				buffer+=c;
				if(i>=scode.length-1){
					tokens.push(buffer);
					buffer="";
				}
			}
		}else if(state==2){ 
			if(c!='.' && (isOp(c) || isNaN(c)) ){
				i--;
				state=0;
				tokens.push(parseFloat(buffer)); 
				buffer="";5
			}else{
				buffer+=c;
				if(i>=scode.length-1){
					tokens.push(parseFloat(buffer));
					buffer="";
				}
			}
		}else if(state==3){
			if(c=='\n'||c=='\r'){
				state=0;
			}
		}else if(state==4){
			if(c=='"'){
				buffer+='"';
				tokens.push(buffer);
				buffer="";
				state=0;
			}else{
				buffer+=c;
			}
		}else if(state==5){
					while(){
			k++;
			if(j[k]==숫자){
				if(j[k-1]=='+')j[k-2]+j[k];
			  if(j[k-1]=='-')j[k-2]-j[k];
	  		if(j[k-1]=='*')j[k-2]*j[k];
	  		if(j[k-1]=='/')j[k-2]/j[k];
			}
			if(j[k]=='+')continue;
			if(j[k]=='-')continue;
			if(j[k]=='*')continue;
			if(j[k]=='/')continue;
			if(j[k]=='=')break;
	}
	addmessage("결과:j[k]!");
}

function onClickRun(){
     scode=document.getElementById("sourcecode").value;

     addMessage("컴파일 중....\n");
     
     scanner();	
     
     dump_token();
     
     math_parser();
}
function math_parser(){
	var math;
	for(var i = 0;i<tokens.length;i++){
		if(isNaN(tokens[i])==false||tokens[i]=='.'){
			math[math.length]=tokens[i];
		}
	}
} 
function dump_token(){
	 document.getElementById("messagebox").value=" "
    var su;
    for(var i=0;i<tokens.length;i++){
        addMessage(tokens[i]);
    }
}

function clearMessageBox(){
    document.getElementById("messagebox").value="";
}

function addMessage(msg){
    document.getElementById("messagebox").value+=msg;
    document.getElementById("messagebox").value+="\n";
}

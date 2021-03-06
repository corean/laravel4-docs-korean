# 템플릿

- [컨트롤러 레이아웃](#controller-layouts)
- [블레이드 템플릿](#blade-templating)
- [다른 블레이드 컨트롤 구조들](#other-blade-control-structures)

<a name="controller-layouts"></a>
## 컨트롤러 레이아웃

Laravel에서 팀플릿을 사용하는 방법중 하나는 컨트롤러 레이아웃을 통해서 사용하는 방법입니다. 컨트롤러에서 `layout` 프로퍼티를 지정하면, 지정된 뷰가 생성되며 액션에서 반환될 가상적인 응답이 됩니다??.(By specifying the `layout` property on the controller, the view specified will be created for you and will be the assumed response that should be returned from actions.)

**컨트롤러에 레이아웃 정의**

    class UserController extends BaseController {
  
  		/**
  		 * The layout that should be used for responses.
  		 */
  		protected $layout = 'layouts.master';
  
  		/**
  		 * Show the user profile.
  		 */
  		public function showProfile()
  		{
  			$this->layout->content = View::make('user.profile');
  		}
  
  	}

<a name="blade-templating"></a>
## 블레이드 템플릿

블레이드는 Laravel에서 제공하는 간단하면서도 강력한 템플릿 엔진 입니다. 컨트롤러 레이아웃과 달리 블레이드는  _템플릿 상속_ 과 _섹션_ 으로 작동됩니다. 모든 블레이드 템플릿은 `.blade.php` 확장자를 사용해야 합니다.

**블레이드 레이아웃 정의**

	<!-- Stored in app/views/layouts/master.blade.php -->

	<html>
		<body>
			@section('sidebar')
				This is the master sidebar.
			@show

			<div class="container">
				@yield('content')
			</div>
		</body>
	</html>

**블레이드 레이아웃 사용**

	@extends('layouts.master')

	@section('sidebar')
		@parent

		<p>이 섹션은 master sidebar에 덧붙혀집니다.</p>
	@stop

	@section('content')
		<p>이 섹션은 body content 입니다.</p>
	@stop

블레이드 레이아웃을 `extend` 하는 뷰는 간단하게 레이아웃의 섹션들을 치환한다는 것에 주목하십시오. 섹션에서 sidebar나 footer 같은 레이아웃 섹션에 컨텐츠를 추가할 수 있게 해주는 `@parent` 명령어를 사용하여, 자식 뷰에 포함할 수 있습니다.

<a name="other-blade-control-structures"></a>
## 다른 블레이드 컨트롤 구조들

**데이터 출력**

	Hello, {{ $name }}.

	The current UNIX timestamp is {{ time() }}.

중괄호 3개로 이루어진 구문를 사용하여 이스케이프한 데이터를 출력할 수 있습니다.:

	Hello, {{{ $name }}}.

**If 문**

	@if (count($records) === 1)
		I have one record!
	@elseif (count($records) > 1)
		I have multiple records!
	@else
		I don't have any records!
	@endif

	@unless (Auth::check())
		You are not signed in.
	@endunless

**반복문**

	@for ($i = 0; $i < 10; $i++)
		The current value is {{ $i }}
	@endfor

	@foreach ($users as $user)
		<p>This is user {{ $user->id }}</p>
	@endforeach

	@while (true)
		<p>I'm looping forever.</p>
	@endwhile

**하위 뷰 인클루드**

	@include('view.name')

**언어 라인 표시**

	@lang('language.line')

	@choice('language.line', 1);

**주석**

	{{-- 이 주석은 렌더링된 HTML에 포함되지 않습니다. --}}

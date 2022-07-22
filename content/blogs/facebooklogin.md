---
title: facebook login
subtitle: 페이스북 간편 로그인 작업
date: 2022-07-22
slug: react-facebook-login
author: han
rating: 5
coverImage: 
---

# facebook 간편 로그인 생성
import FacebookLogin from '@greatsumini/react-facebook-login';

        <div>
        <FacebookLogin
            appId={appId}
            onProfileSuccess={(response) => {
                this.onSuccessFacebook(response);
            }}
            onFail={() => {
                alert('fail');
            }}
            render={(renderProps) => (
                <button onClick={renderProps.onClick}
                disabled={renderProps.disabled}
                style={{
                    padding: '0',
                    width: '220px',
                    height: '45px',
                    'line-height': '44px',
                    'color': '#000000',
                    'background-color': '#3b5998',
                    border: '1px solid transparent',
                    'border-radius': '3px',
                    'font-size': '14px',
                    'font-weight': 'bold',
                    'text-align': 'center',
                    'cursor': 'pointer',
                    'color': 'white'
                }}>
                페이스북 로그인
                </button>
            )}           
        />
    </div>

로 생성

참고 : https://sumini.dev/guide/016-react-facebook-login/

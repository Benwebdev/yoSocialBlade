# yoSocialBlade

@benline/yoSocialBlade 

Usage: 

``` typescript 
blSocialBlade('UC-lHJZR3Gqxm24_Vd_AJ5Yw').then(function (metrics) {
    console.log(metrics)
})


Full script: 

``` typescript

import * as fakeUa from "fake-useragent";
import { get } from "request";
// import * as cheerio from "cheerio";
import { load } from "cheerio";
import * as _ from "lodash";

// SocialBlade API that doesn't need auth or a key, DOM might change: 
// It works as of 30/01/2021 
// Returns promise. 

const blSocialBlade = function (channel_id) {
    // Return new promise
    return new Promise(function (resolve, reject) {
        const url = 'https://socialblade.com/youtube/channel/' + channel_id
        const headers = {
            'User-Agent': fakeUa()
        }
        // Do async query
        get({ url: url, headers: headers }, function (err, r, body) {
            if (!err) {
                const $ = load(body)

                const grade = $('body > div:nth-child(19) > div#socialblade-user-container:nth-child(1) > div#socialblade-user-content:nth-child(1) > div:nth-child(1) > div:nth-child(1) > div:nth-child(1)').text()


                let result = {
                    grade: grade
                }

        //Remove all unnecessary chars
        resolve(_.mapValues(result, v => v.replace(/[\n,]/g, '').trim()))
    } else {
        reject(err);
    }
        })
    })
}

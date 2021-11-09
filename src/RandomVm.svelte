<script>
	import moment from "moment-timezone";
    import { onMount } from "svelte";
    import { writable, derived } from 'svelte/store'
    import fetchJsonp from 'fetch-jsonp';

    function getAdjustedDateTime(meeting_day, meeting_time, meeting_time_zone) {
        let adjustedMeetingDay = parseInt(meeting_day) === 1 ? 7 : parseInt(meeting_day) - 1
        let meeting_date_time_obj;

        if (!meeting_time_zone) {
            meeting_time_zone = "UTC";
        }

        // Get an object that represents the meeting in its time zone
        meeting_date_time_obj = moment.tz(meeting_time_zone).set({
            hour: meeting_time.split(":")[0],
            minute: meeting_time.split(":")[1],
            second: 0
        }).isoWeekday(adjustedMeetingDay);

        // Convert meeting to target (local) time zone
        meeting_date_time_obj = meeting_date_time_obj.clone().tz(moment.tz.guess());

        var now = moment.tz(moment.tz.guess())
        if (now > meeting_date_time_obj) {
            meeting_date_time_obj.add(1, 'weeks');
        }

        return meeting_date_time_obj;
    }


    export const meetings = writable([]);
    onMount(async () => {
        fetchJsonp('https://bmlt.virtual-na.org/main_server/client_interface/jsonp/?switcher=GetSearchResults&data_field_key=weekday_tinyint,start_time,time_zone,meeting_name,comments')
            .then(function(response) {
                return response.json()
            }).then(function(json) {
            meetings.set(json);
        }).catch(function(ex) {
            console.log('parsing failed', ex)
            return [];
        })
    });

    export const meetingNames = derived(meetings, ($meetings) => {
        let results = [];
        if ($meetings){
            for (let x in $meetings) {
                let meeting_day = $meetings[x]['weekday_tinyint'];
                let meeting_time = $meetings[x]['start_time'];
                let meeting_time_zone = $meetings[x]['time_zone'];
                let start = getAdjustedDateTime(meeting_day, meeting_time, meeting_time_zone);
                let now = moment.tz(moment.tz.guess())
                if (start.diff(now, 'minutes') >= 0 && start.diff(now, 'minutes') <= 30) {
                    results.push({"name": $meetings[x]['meeting_name'], "start": start._d.toString(), "link": $meetings[x]['comments']})
                }
            }

            let meeting = results[Math.floor(Math.random() * results.length)];
            let a = {name: "The Misfits of NA", start: "Mon Nov 08 2021 18:00:00 GMT-0500 (Eastern Standard Time)", link: "https://zoom.us/j/535829413"}
            console.log('a obj', a)
            console.log('meeting obj', meeting)
            return a;

        }
        return [];
    });

</script>

<main>
    <h1>Meeting Names</h1>
    <ul>
        {#each Object.entries($meetingNames) as [key, val]}
            <li>{key}</li>
            <li>{val}</li>
        {/each}
    </ul>
    <ul>
        <!--{#each $meetingNames as name}-->
        <!--    <li>{name}</li>-->
        <!--{/each}-->
    </ul>
</main>

<style>
    main {
        margin: 2vh 1vw;
    }
    .full-width {
        width: 95%;
    }
    .data {
        font-size: 20px;
    }

</style>
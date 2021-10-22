<script context="module">
    export const fetchData = async (url, setting) => {
        const response = await fetch(url, { 
            headers: {'User-Agent': 'request'},
            ...setting
        })
        return await response.json();
    }
</script>  


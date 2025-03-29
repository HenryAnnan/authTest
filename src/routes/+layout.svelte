<script>
    import { invalidate } from '$app/navigation'
    import { onMount } from 'svelte'

    let { data, children } = $props()
    let { session, supabase } = $derived(data)

    onMount(() => {
        const { data } = supabase.auth.onAuthStateChange((_, newSession) => {
            if (newSession?.expires_at !== session?.expires_at) {
                invalidate('supabase:auth')
            }
        })

        return () => data.subscription.unsubscribe()
    })
</script>

<link
        rel="stylesheet"
        href="https://cdn.jsdelivr.net/npm/@picocss/pico@2/css/pico.min.css"
>

{@render children()}
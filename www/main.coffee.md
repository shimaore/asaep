    ($ document).ready ->

      hoodie = new Hoodie()

      signed_in = ->
        ($ 'html').removeClass 'signed-out'
        ($ 'html').addClass 'signed-in'

      signed_out = ->
        ($ 'html').removeClass 'signed-in'
        ($ 'html').addClass 'signed-out'

      signed_class = ->
        if hoodie.account.username?
          signed_in()
          ($ 'input.username').val hoodie.account.username
          ($ 'span.username').html hoodie.account.username
          $.mobile.changePage '#listview'
        else
          signed_out()
          ($ 'input.username').val ''
          ($ 'span.username').html ''
          $.mobile.changePage '#'

      do signed_class

      ($ '#signup form').submit (e) ->
        e.preventDefault()
        username = $(@).find('.username').val().trim()
        password = $(@).find('.password').val().trim()
        hoodie.account.signUp(username,password)
        signed_class()
        false

      ($ '#signin form').submit (e) ->
        e.preventDefault()
        username = $(@).find('.username').val().trim()
        password = $(@).find('.password').val().trim()
        hoodie.account.signIn(username,password)
        false

      ($ '.signout').click (e) ->
        e.preventDefault()
        hoodie.account.signOut()
        false

      hoodie.account.on 'signin', signed_class
      hoodie.account.on 'signout', signed_class

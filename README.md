# Telegram-Bot

Perl module to make creating new Telegram bots easy.

How easy? This easy:

    package MyBot;
    
    use Mojo::Base 'Telegram::Bot::Brain';

    has token => 'YOURTOKENHERE';

    sub _hello_for_me {
      my ($self, $msg) = @_;
      # look for the word 'hello' with or without a leading slash
      if ($msg->text =~ m{/?hello}i) {
        return 1;
      }
      return 0;
    }

    sub _be_polite {
      my ($self, $msg) = @_;

      # is this a 1-on-1 ?
      if ($msg->chat->is_user) {
        $self->send_message_to_chat_id($msg->chat->id, "hello there");
      }
      # group chat
      else {
        $self->send_message_to_chat_id($msg->chat->id, "hello to everyone!");
      }
    }

    sub init {
      my $self = shift;
      $self->add_listener(\&_hello_for_me,
                          \&_be_polite);
 
    }

    1;
    
Now just:

    perl -MMyBot -E 'MyBot->new->think'
    
and you've got yourself a stew, baby! Or a bot, anyway.
